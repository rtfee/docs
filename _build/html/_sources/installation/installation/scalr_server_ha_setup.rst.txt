Scalr Server HA Replication
===========================

Setting up Replication for the Scalr Database
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you are using the built-in MySQL server that comes with Scalr, you can use the kickstart-replication script to setup replication to your slave database. 

Scalr should already be installed, if you have not installed Scalr yet, click here. Make all necessary changes to scalr-server.rb and make sure to specify that master server is configured to be used as database server.

On the master database, make sure your scalr-server-local.rb has the following set:

.. code-block:: shell

   # Enable the MySQL component
   mysql[:enable] = true

   # Set unique ID of this MySQL server
   mysql[:server_id] = 1

   # Enable binary log needed for replication
   mysql[:binlog] = true

   # Allow remote root access is required by the kickstart-replication script
   mysql[:allow_remote_root] = true

   # Specify which IP the slave will connect from to grant the correct privileges to the replication user
   mysql[:repl_allow_connections_from] = 'SLAVE-IP'

On the slave database, make sure your scalr-server-local.rb has the following set:

.. code-block:: shell

   # Enable the MySQL component
   mysql[:enable] = true
 
   # Set unique ID of this MySQL server
   mysql[:server_id] = 2
 
   # Enable binary log needed for replication
   mysql[:binlog] = true
 
   # Allow remote root access is required by the kickstart-replication script
   mysql[:allow_remote_root] = true

Run scalr-server-ctl reconfigure on server 1.
Run scalr-server-ctl reconfigure on server 2.

Start replication by executing: 

.. code-block:: shell

   /opt/scalr-server/bin/kickstart-replication IP-OF-MASTER:3306 IP-OF-SLAVE:3306
