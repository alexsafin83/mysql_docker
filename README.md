# mysql_docker
Best docker set up for MySQL out there

## 1. What you will get
You will get
- mysql 8.4
- a docker network `localdev`
- mysql data persistence

## 2. Configuration
### 2.1 Network
There's a local network with id=localdev preconfigured in `docker-compose.yaml`.
Add it under "networks:" in your service's docker compose config or change it to your needs.\
Your service and the mysql container need to use the same network to be reachable for each other.

### 2.2 Volumes
#### 2.2.1 /dump
This volume is linked to `/docker-entrypoint-initdb.d/` inside mysql container.
Here you can add your startup sql scripts, to initialize your database. Mostly it's a Database dump file to create db structure and insert data.

#### 2.2.2 mysql_db_persistence
This is a named volume which allows to persist container's database data (which is located in `/var/lib/mysql` inside container).
This is needed to have your db data restored, even if you stop and remove your containers to start them later.\
\
If you intend to run another mysql container in parallel, then give your second db persistent volume another name, like 'mysql_db_persistence2'.\
Also then you may need to configure the networks, as this set up is defining the network via docker-compose.yaml, to be used as network of other services,
which need to access the database. The same network must not be redefined by another set up.

If you need to reinitialize the database, don't forget to delete the volume via
```
docker volume rm mysql_db_persistence
```

## 3. Good to know
#### 3.1 Env MYSQL_ROOT_PASSWORD will be ignored if you use your own database file. It won't change any passwords.
