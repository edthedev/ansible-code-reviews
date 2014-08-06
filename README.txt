
Download Package Files
-----------------------

Bring up the Virtual Machine::

	vagrant up

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



Create a new ReviewBoard site::

    vagrant ssh
    # rb-site install /var/www/reviews.example.com
    rb-site install /home/vagrant/example.com

Visit your ReviewBoard site::

Visit localhost to check configuration status:
http://127.0.0.1:8080

Note that http://localhost:8080/ will not work, because Phabricator demands a URL with a dot in it.

Troubleshooting
----------------
TODO: Find the ReviewBoard logs
