
Download Package Files
-----------------------

Download the latest Gerrit binary in files/gerrit.war.::

	firefox https://gerrit-releases.storage.googleapis.com/index.html
	mv ~/Downloads/gerrit-*.war source_files/gerrit.war


Bring up the Virtual Machine::

	vagrant up

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

Start Gerrit::

    vagrant ssh
    sudo su gerrit2

    # Initialize Gerrit
    # Don't do this step. Use the Ansible gerrit.config instead.
    # java -jar /home/gerrit2/gerrit.war init -d /home/gerrit2

    # Start Gerrit
    /home/gerrit2/bin/gerrit.sh start
