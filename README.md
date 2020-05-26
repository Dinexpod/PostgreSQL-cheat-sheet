# PostgreSQL-cheat-sheet
Query cheat-sheet about postgres and just sql

========================================================================================================================================

install postgres: <br/>
__~$ sudo apt-get update && sudo apt-get install postgresql-12__

start pg: <br/>
__~$ sudo pg_ctlcluster 12 main start__

status pg: <br/>
__~$ sudo service postgresql status__

go to pg console some db: <br/>
__~$ sudo -u username psql -d db_name__
__\q__ - exit from console

========================================================================================================================================

create db: <br/>
__=# create database db_name;__

select current catalog: <br/>
__=#  select current_catalog;__

change password for user: <br/>
__=#  alter user user_name with password 'password';__

========================================================================================================================================

create empty table table: <br/>
__=#  create table table_name __

create table with argumrnts: <br/>
__=#  __
