# docker-compose

```(sh)
docker-compose -f docker-compose.yml -f env.yml -f volume.yml ps
docker-compose -f docker-compose.yml -f env.yml -f volume.yml build
docker-compose -f docker-compose.yml -f env.yml -f volume.yml up -d <service name>

# if you're using linux add an alias to .bashrc similar to below (configure path carefully)
echo "alias srvc='docker-compose -f ~/srvc/docker-compose.yml -f ~/srvc/env.yml -f ~/srvc/volume.yml'" >> ~/.bashrc
echo "alias srvc-logs='srvc logs -f'" >> ~/.bashrc
source ~/.bashrc

# once you're done with above linux commands, you can use below commands
srvc ps
srvc up -d pg
srvc-logs pg

```

### available services

- **mongo**
- **pg (Postgres)**
- **kc (Keycloak)**


# pg

**Installing and connecting to psql**

```(bash)

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list' 

wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - 

sudo apt update

sudo apt install postgresql-client-13

psql -U <user> -d <db> -h <host>

```

**Creating database and granting access (Inside psql shell)**

```(psql)
# if you want to remove existing db
drop database <dbname>

create new database
create database <dbname>

# to create user
create user <username> with encrypted password 'password';

# revoke access to all the other databases
revoke CONNECT on DATABASE template1 from PUBLIC;
revoke CONNECT on DATABASE template0 from PUBLIC;
revoke CONNECT on DATABASE admin from PUBLIC;
revoke CONNECT on DATABASE postgres from PUBLIC;

# grant privileges only to the required database
grant ALL privileges on database <dbname> to <username>;

```


# mongo

**Installing and connecting to mongo shell**

```(sh)

wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -

echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list 

sudo apt update

sudo apt install mongodb-org-shell

# if you need to use mongodump and mongorestore install mongodb-org-tools also

sudo apt install mongodb-org-tools

mongo -u <user> -p

```

**Creating database and granting access (Inside psql shell)**

```(javascript)
db.createUser({
    user: 'user',
    pwd: passwordPrompt(),
    roles: [{role: 'dbOwner', db: 'dbname'}]
})

# connection string for connecting to a specific db with authentication
# mongo -u <user> --authenticationDatabase <dbname> -p
```