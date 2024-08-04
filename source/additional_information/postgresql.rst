############
PostgreSQL
############


PostgreSQL is a enterprise grade open source database.
http://www.postgresql.org/

Backup
------

The following assumes the database username is cclpbx and the
database to backup is cclpbx. Make sure you have the database
password ready.

| ``su postgres``
| ``pg_dump -U cclpbx cclpbx -b -v -f /tmp/cclpbx.sql``

Restore
-------

Assuming username cclpbx and database cclpbx

``psql -U cclpbx -d cclpbx -f cclpbx.sql``

Console
-------

| ``su postgres``
| ``psql``

list the databases

``\l``

connect to the database

``\connect cclpbx``

or

``\c cclpbx``

list tables

``\d``

drop the database

``DROP DATABASE cclpbx;``

create the database

``CREATE DATABASE cclpbx;``

Links
-----

http://www.mkyong.com/database/backup-restore-database-in-postgresql-pg_dumppg_restore/

http://www.postgresql.org/docs/9.1/static/backup.html
