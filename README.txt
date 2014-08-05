
Download Package Files
-----------------------

Double check that the Phabricator install shell script is up to date::

   https://secure.phabricator.com/book/phabricator/article/installation_guide/ 

Bring up the Virtual Machine::

	vagrant up

Run the Phabricator install shell script::

    vagrant ssh
    cd /home/vagrant
    source /vagrant/install_rhel-derivs.sh

    ...Answer prompts...
    ...Script will attempt to sudo yum install, as needed...
    ...Many installs occur...

    Complete!
    Please remember to start the httpd with: /etc/init.d/httpd start
    Please remember to start the mysql server: /etc/init.d/mysqld start
    Press RETURN to continue, or ^C to cancel.

    ...some Git stuff...

    ...Reminder to visit the configuration guide to finish configuration.

Configure Apache::

    sudo vi /etc/httpd/conf/httpd.conf

Example Apache config::
    
    <VirtualHost *>
      # Change this to the domain which points to your host.
      ServerName localhost

      # Change this to the path where you put 'phabricator' when you checked it
      # out from GitHub when following the Installation Guide.
      #
      # Make sure you include "/webroot" at the end!
      DocumentRoot /vagrant

      RewriteEngine on
      RewriteRule ^/rsrc/(.*)     -                       [L,QSA]
      RewriteRule ^/favicon.ico   -                       [L,QSA]
      RewriteRule ^(.*)$          /index.php?__path__=$1  [B,L,QSA]
    </VirtualHost>

Start httpd and mysql::

    /etc/init.d/httpd start
    /etc/init.d/mysqld start

Visit the configuration guide:
http://www.phabricator.com/docs/phabricator/article/Configuration_Guide.html

Create the Postgres Database::

    # Connect via SSH to your VM
    vagrant ssh
    # Start and init postgres
    service postgresql initdb
    sudo service postgresql start
    # Start a Postgres Psql shell
    sudo -u postgres psql postgres
    # Create the gerrit database user and tables
    createuser --username=postgres -RDIElPS gerrit2
    createdb --username=postgres -E UTF-8 -O gerrit2 reviewdb
    # Press Ctrl+D to exit the Psql shell.

Create a Git repository::

    mkdir -r /home/gerrit2/git
    cd /home/gerrit2/git
    git --bare init example.git

Start Gerrit::

    vagrant ssh
    sudo su gerrit2

    # Initialize Gerrit
    # Don't do this step. Use the Ansible gerrit.config instead.
    java -jar /home/gerrit2/gerrit.war init -d /home/gerrit2/gerrit_site

    # Start Gerrit
    export site_path=/home/gerrit2
    export GERRIT_SITE=/home/gerrit2/gerrit_site
    /home/gerrit2/gerrit_site/bin/gerrit.sh start


Troubleshooting
----------------


Gerrit log files are at::

    /home/gerrit2/gerrit_site/logs
