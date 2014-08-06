
Download Package Files
-----------------------

Bring up the Virtual Machine::

	vagrant up

If anything needs to be corrected in playbook.yml, you can re-run the provision step with::

    vagrant provision

Create a new ReviewBoard site::

    vagrant ssh
    rb-site install /home/vagrant/localhost

    ... Choose some options ...
        - Domain: localhost
        - Database: sqlite
        - Web server: Apache and mod_wsgi

    The site has been installed in /home/vagrant/localhost

    Sample configuration files for web servers and cron are available
    in the conf/ directory.

    You need to modify the ownership of the following directories and
    their contents to be owned by the web server:
        * /home/vagrant/localhost/htdocs/media/uploaded
        * /home/vagrant/localhost/htdocs/media/ext
        * /home/vagrant/localhost/data

    For more information, visit:

    http://www.reviewboard.org/docs/manual/dev/admin/installation/creating-sites/


Configure apache::

    sudo chown -R apache:apache /home/vagrant/localhost
    sudo chmod 755 /home/vagrant

    vi /etc/httpd/conf/httpd.conf
    Include /home/vagrant/localhost/conf/apache-wsgi.conf

Restart Apache::

    sudo service httpd restart

Create at least one local Git repository::

    mkdir /home/vagrant/repos
    sudo chown -R vagrant:apache /home/vagrant/repos
    git init --bare /home/vagrant/repos/NetIDApps.git

Add the Vagrant SSH to your SSH Config::

    vagrant ssh-config >> ~/.ssh/config

Checkout a local copy of your repository,
and add a 'review' remote::

    git clone https://git-sdg.cites.illinois.edu/Scratch/delaport/NetIDApps.git
    cd NetIDApps
    git remote add review vagrant@default:/home/vagrant/repos/NetIDApps.git
    git push review

    Counting objects: 2579, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (1117/1117), done.
    Writing objects: 100% (2579/2579), 7.20 MiB | 0 bytes/s, done.
    Total 2579 (delta 1495), reused 2443 (delta 1426)
    To vagrant@default:/home/vagrant/repos/NetIDApps.git
     * [new branch]      master -> master

Visit your ReviewBoard site::

Visit localhost to check configuration status:
http://127.0.0.1:8080

Note that http://localhost:8080/ will not work, because Phabricator demands a URL with a dot in it.

Troubleshooting
----------------
TODO: Find the ReviewBoard logs
