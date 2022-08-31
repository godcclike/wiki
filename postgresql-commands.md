# Postgres SQL Commands

## Link for command reference
https://www3.ntu.edu.sg/home/ehchua/programming/sql/PostgreSQL_GetStarted.html

## To login to database
```Shell
sudo su postgres
psql
```

## Common commands to run when logged in
| Syntax | Description |
| ----------- | ----------- |
| `\l`                      | to show all databases |
| `\c <database name>`      | to login inside a specific database |
| `\dt`                     | to display all tables in the database |

## To create database and grant all priviledges
```Shell
CREATE DATABASE omsdbah2 WITH TEMPLATE = template0 ENCODING 'UTF8' OWNER=omsuser;
GRANT ALL PRIVILEGES ON DATABASE omsdbah2 TO omsuser;
```

## To stop / start DB
| Syntax | Description |
| ----------- | ----------- |
| `cd /usr/lib/systemd/system/` |
| `postgresql-12.service` |
| `systemctl status postgresql-12.service`      | check the service status |
| `sudo service postgresql-12.service stop`     | Stop the service |
| `sudo service postgresql-12.service start`    | Start the service | 
| `sudo service postgresql-12.service restart`  | Stop and restart the service |
| `sudo service postgresql-12.service reload` | to reload the services

## Force drop a database
```Shell
psql -h localhost postgres postgres
password = Platinum@1

ALTER DATABASE omsdbah2 CONNECTION LIMIT 0;

SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE datname = 'omsdbah2';

drop database omsdbah2;
```
