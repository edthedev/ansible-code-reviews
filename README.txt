
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

    sudo chown -R www:www /home/vagrant/localhost
    sudo chmod 755 /home/vagrant

    sudo cp /etc/httpd/conf/httpd.conf ~
    sudo cp /home/vagrant/localhost/conf/apache-wsgi.conf /etc/httpd/conf/httpd.conf

    vi /etc/httpd/conf/httpd.conf
    # Comment out this bit: 
    # WSGIPassAuthorization On

    Include /home/vagrant/localhost/conf/apache-wsgi.conf

Restart Apache::

    sudo service httpd restart

Visit your ReviewBoard site::

Visit localhost to check configuration status:
http://127.0.0.1:8080

Note that http://localhost:8080/ will not work, because Phabricator demands a URL with a dot in it.

Troubleshooting
----------------
TODO: Find the ReviewBoard logs
