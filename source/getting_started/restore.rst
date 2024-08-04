*********
Restore
*********

|

It's always good to have a restore method of a backup in place.  Here are the steps to a basic restore method with cclpbx.

.. Note:: It is important to know if your installation is from package or source as the paths are different for FreeSWITCH.  Always test the backups and restore methods on test machines first.

*  To create the script use an editor such as vi or nano.
*  Copy/Paste from the code block below and save the file as cclpbx-restore.sh
*  Replace zzz with your database password
*  chmod + x cclpbx-restore.sh and then run the script ./cclpbx-restore.sh
*  edit the script as needed and run this script from the server you are restoring on.


::

 #!/bin/sh
 now=$(date +%Y-%m-%d)
 ssh_server=x.x.x.x
 database_host=127.0.0.1
 database_port=5432
 export PGPASSWORD="zzz"

 #run the remote backup
 ssh -p 22 root@$ssh_server "nice -n -20 /etc/cron.daily/./cclpbx-backup.sh"

 #delete freeswitch logs older 7 days
 find /var/log/freeswitch/freeswitch.log.* -mtime +7 -exec rm {} \;

 #synchronize the backup directory
 #rsync -avz -e 'ssh -p 22' root@$ssh_server:/var/backups/cclpbx /var/backups
 rsync -avz -e 'ssh -p 22' root@$ssh_server:/var/backups/cclpbx/postgresql /var/backups/cclpbx
 rsync -avz -e 'ssh -p 22' root@$ssh_server:/var/www/cclpbx /var/www
 rsync -avz -e 'ssh -p 22' root@$ssh_server:/etc/cclpbx /etc
 find /var/backups/cclpbx/postgresql -mtime +2 -exec rm {} \;

 rsync -avz -e 'ssh -p 22' root@$ssh_server:/etc/freeswitch/ /etc
 rsync -avz -e 'ssh -p 22' root@$ssh_server:/var/lib/freeswitch/storage /var/lib/freeswitch
 rsync -avz -e 'ssh -p 22' root@$ssh_server:/var/lib/freeswitch/recordings /var/lib/freeswitch
 rsync -avz -e 'ssh -p 22' root@$ssh_server:/usr/share/freeswitch/scripts /usr/share/freeswitch
 rsync -avz -e 'ssh -p 22' root@$ssh_server:/usr/share/freeswitch/sounds /usr/share/freeswitch
 
 echo "Restoring the Backup"
 #extract the backup from the tgz file
 #tar -xvpzf /var/backups/cclpbx/backup_$now.tgz -C /

 #remove the old database
 psql --host=$database_host --port=$database_port  --username=cclpbx -c 'drop schema public cascade;'
 psql --host=$database_host --port=$database_port  --username=cclpbx -c 'create schema public;'
 #restore the database
 pg_restore -v -Fc --host=$database_host --port=$database_port --dbname=cclpbx --username=cclpbx /var/backups/cclpbx/postgresql/cclpbx_pgsql_$now.sql

 #restart freeswitch
 service freeswitch restart
 echo "Restore Complete";


