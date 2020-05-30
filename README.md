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

create empty table: <br/>
__=#  create table table_name__

delete table: <br/>
__=#  drop table table_name__

create table with argumrnts: <br/>
__=#  create table table_name (
id serial primary key,
name text,
age integer
);__

insert in table: <br/>
__=# insert into contragents (name)  select 'Sam' from  generate_series(50001, 5000000);__

update attribute value in table: <br/>
__=# update friends set id = 2 where id = 1;


============================================================

serial use instead of this: <br/>
__=#  CREATE SEQUENCE user_id_seq;__

__=#  CREATE TABLE user (
    user_id smallint NOT NULL DEFAULT nextval('user_id_seq')
);__

__=#  ALTER SEQUENCE user_id_seq OWNED BY user.user_id;__

============================================================

constraint on attributes: <br/>
__=#  create table table_name <br/>
(<br/>
attr1 integer CHECK ( attr1 = 1 OR attr1 = 2 ),<br/>
attr2 integer CHECK ( attr2 >= 3 AND attr2 <= 5 ),<br/>
attr3 text NOT NULL,<br/>
attr4 text CHECK (attr4 IS NOT NULL),<br/>
attr5 integer UNIQUE,<br/>
attr6 text PRIMARY KEY,<br/>
CONSTRAINT uniq_mix UNIQUE ( attr1, attr2 ),<br/>
PRIMARY KEY (attr6, attr7, ... )<br/>
);__

refferenses on another table attribute: <br/>
__=#  create table table_name ( id integer REFERENCES another_table_name ( id ) );__<br/>
__=#  create table table_name ( id integer REFERENCES another_table_name );__<br/>
__=#  create table table_name ( id integer REFERENCES another_table_name ON DELETE CASCADE );__

create empty table: <br/>
__=# create table users (
id serial primary key, 
friend_id integer REFERENCES friends
ON DELETE CASCADE 
ON UPDATE CASCADE);

============================================================

ALTER

 using ALTER: <br/>
__=# ALTER TABLE table_name ADD COLUMN__<br/>
__=# ALTER TABLE users RENAME COLUMN mon TO mom;
__=# ALTER TABLE table_name DROP COLUMN__ <br/>
__=# ALTER TABLE table_name ADD CHECK (age < 40)__<br/>
__=# ALTER TABLE table_name ADD CONSTRAIN__<br/>
__=# ALTER TABLE aircrafts ALTER COLUMN speed SET NOT NULL;__<br/>
__=# ALTER TABLE seats RENAME CONSTRAINT seats_fare_conditions_fkey TO seats_fare_conditions_code_fkey;__<br/>

__=# ALTER TABLE seats<br/>
ALTER COLUMN fare_conditions SET DATA TYPE integer<br/>
USING ( CASE WHEN fare_conditions = 'Economy' THEN 1<br/>
WHEN fare_conditions = 'Business' THEN 2<br/>
ELSE 3 END );__

============================================================

VIEW

create view: 
__=# CREATE VIEW seats_by_fare_cond AS<br/>
SELECT a.model,<br/>
s.aircraft_code,<br/>
s.fare_conditions,<br/>
count( * ) AS num_seats<br/>
FROM seats<br/>
GROUP BY aircraft_code, fare_conditions<br/>
ORDER BY aircraft_code, fare_conditions;__

select from view: <br/>
__=# select * from view_name;__

create materialized view: <br/>
__=# create materialized  view parents_by_user_m as <br/>
select id, name, mom, dad from users <br/>
group by id, name <br/>
order by id, name;__

refresh materialized view: <br/>
__=# REFRESH MATERIALIZED VIEW view_name;__

===========================================================================

SCHEMAS

show schema: <br/>
__=#  show search_path;__

change schema: <br/>
__=#  set search_path = schema_name;__

show schema name: <br/>
__=#  select current_schema;__

===========================================================================

QUERIES

__=# SELECT * FROM aircrafts WHERE model LIKE 'Airbus';__<br/>
__=# SELECT * FROM aircrafts WHERE model LIKE 'Airbus%';__<br/>
__=# SELECT * FROM aircrafts WHERE model LIKE '%Airbus';__<br/>
__=# SELECT * FROM aircrafts WHERE model LIKE '%Airbus%';__<br/>
__=# SELECT * FROM aircrafts WHERE model NOT LIKE '%Airbus%';__<br/>
__=# SELECT * FROM airports WHERE airport_name LIKE '___';__<br/>
__=# SELECT * FROM aircrafts WHERE model ~ '^(A|Boe)';__<br/>
__=# SELECT * FROM aircrafts WHERE model !~ '300$';__<br/>
__=# SELECT * FROM aircrafts WHERE range BETWEEN 3000 AND 6000;__<br/>
__=# SELECT model, range, range / 1.609 AS miles FROM aircrafts;__<br/>
__=# SELECT model, range, round( range / 1.609, 2 ) AS miles FROM aircrafts;__<br/>
__=# SELECT * FROM aircrafts ORDER BY range DESC;__<br/>
__=# SELECT * FROM aircrafts ORDER BY range;__<br/>
__=# SELECT * FROM aircrafts GROUP BY range;__<br/>
__=# SELECT DISTINCT timezone FROM airports ORDER BY 1;__ // distinct - without duplicates<br/>
__=# SELECT name, city, longitude FROM airports ORDER BY longitude DESC LIMIT 3 OFFSET 3;__ // offset - skip first 3<br/>
__=# SELECT model, range, <br/>
CASE WHEN range < 2000 THEN 'Ближнемагистральный' <br/>
WHEN range < 5000 THEN 'Среднемагистральный'<br/>
ELSE 'Дальнемагистральный' <br/>
END AS type FROM aircrafts ORDER BY model;__<br/>
__=# SELECT count( * ) FROM airports a1 JOIN airports a2 ON a1.city <> a2.city;__<br/>
__=# SELECT count( * ) FROM airports a1 CROSS JOIN__<br/>
__=# __<br/>
__=# __<br/>

===========================================================================

AGREGATION

SELECT avg( total_amount ) FROM bookings;
SELECT min( total_amount ) FROM bookings;
SELECT max( total_amount ) FROM bookings;

SELECT city, count( * )
FROM airports
GROUP BY city
HAVING count( * ) > 1;
