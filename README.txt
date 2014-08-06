
Download Package Files
-----------------------

Bring up the Virtual Machine::

	vagrant up

If anything needs to be corrected in playbook.yml, you can re-run the provision step with::

    vagrant provision

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
