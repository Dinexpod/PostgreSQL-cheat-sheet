# PostgreSQL-cheat-sheet
Query cheat-sheet about postgres and just sql

============================================================

install postgres: <br/>
__~$ sudo apt-get update && sudo apt-get install postgresql-12__

start pg: <br/>
__~$ sudo pg_ctlcluster 12 main start__

status pg: <br/>
__~$ sudo service postgresql status__

go to pg console some db: <br/>
__~$ sudo -u username psql -d db_name__
__\q__ - exit from console

============================================================

create db: <br/>
__=# create database db_name;__

select current catalog: <br/>
__=#  select current_catalog;__

change password for user: <br/>
__=#  alter user user_name with password 'password';__

============================================================

create empty table table: <br/>
__=#  create table table_name__

create table with argumrnts: <br/>
__=#  create table table_name (
id serial primary key,
name text,
age integer
);__

============================================================

serial use instead of this: <br/>
__=#  CREATE SEQUENCE user_id_seq;__

__=#  CREATE TABLE user (
    user_id smallint NOT NULL DEFAULT nextval('user_id_seq')
);__

__=#  ALTER SEQUENCE user_id_seq OWNED BY user.user_id;__

============================================================

constraint on attributes: <br/>
__=#  create table table_name 
(
attr1 integer CHECK ( attr1 = 1 OR attr1 = 2 ),
attr2 integer CHECK ( attr2 >= 3 AND attr2 <= 5 ),
attr3 text NOT NULL,
attr4 text CHECK (attr4 IS NOT NULL),
attr5 integer UNIQUE,
attr6 text PRIMARY KEY,
CONSTRAINT uniq_mix UNIQUE ( attr1, attr2 ),
PRIMARY KEY (attr6, attr7, ... )
);__

refferenses on another table attribute: <br/>
__=#  create table table_name ( id integer REFERENCES another_table_name ( id ) );__

__=#  create table table_name ( id integer REFERENCES another_table_name );__

