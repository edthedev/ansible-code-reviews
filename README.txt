
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

    <Location />
        Order deny,allow
        Allow from all
    </Location>

Start httpd and mysql::

    /etc/init.d/httpd start
    /etc/init.d/mysqld start

Visit the configuration guide:
http://www.phabricator.com/docs/phabricator/article/Configuration_Guide.html

Setup Database::

    /usr/bin/mysqladmin -u root password 'new-password'
    # nevermind: /usr/bin/mysqladmin -u root -h localhost.localdomain password 'new-password'


    cd /vagrant/phabricator
    ./bin/config set mysql.host localhost
    # ./bin/config set mysql.port value
    ./bin/config set mysql.user root
    ./bin/config set mysql.pass new-password

    ./bin/storage upgrade --force --user root --password 'new-password'


Visit localhost to check configuration status:
http://localhost:8080/

Troubleshooting
----------------
TODO: Find the Phab logs

