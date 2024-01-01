# oracle

# XE
## build
```bash
git clone git@github.com:oracle/docker-images.git
cd docker-images/OracleDatabase/SingleInstance/dockerfiles

./buildContainerImage.sh -v 18.3.0 -t oracle:18.3.0 -e -i -o '--build-arg SLIMMING=false'
./buildContainerImage.sh -v 18.4.0 -t oracle:18.4.0 -x -i -o '--build-arg SLIMMING=false'
./buildContainerImage.sh -v 19.3.0 -t oracle:19.3.0 -e -i -o '--build-arg SLIMMING=false'
./buildContainerImage.sh -v 21.3.0 -t oracle:21.3.0 -e -i -o '--build-arg SLIMMING=false'
```

## docker
```bash
mkdir -p /opt/crudapi/oracle
chmod 777 /opt/crudapi/oracle

docker rm -f oracle
docker run --name oracle \
    --restart=always \
    -p 1521:1521 -p 5500:5500 \
    -e ORACLE_PWD=Crudapi1521 \
    -e ORACLE_CHARACTERSET=AL32UTF8 \
    -e ENABLE_ARCHIVELOG=true \
    -v /opt/crudapi/oracle:/opt/oracle/oradata \
    -d oracle:18.4.0

docker logs -f oracle
``` 

## schema
```bash
create tablespace tbs_crudapi datafile '/opt/oracle/oradata/XE/tbs_crudapi.dbf' size 2048M;
drop user crudapi;
create user crudapi identified by crudapi;
alter user crudapi default tablespace tbs_crudapi;
grant connect, resource, dba to crudapi;
```

## docs
[Oracle Database container images](https://github.com/oracle/docker-images/blob/main/OracleDatabase/SingleInstance/README.md)


# free
## docker
```bash
mkdir -p /opt/crudapi/oracle
chmod 777 /opt/crudapi/oracle

docker rm -f oracle
docker run --name oracle \
    --restart=always \
    -p 1521:1521 -p 5500:5500 \
    -e ORACLE_PWD=Crudapi1521 \
    -e ORACLE_CHARACTERSET=AL32UTF8 \
    -e ENABLE_ARCHIVELOG=true \
    -v /opt/crudapi/oracle:/opt/oracle/oradata \
    -d container-registry.oracle.com/database/free

docker logs -f oracle
``` 

### schema
```bash
sqlplus / as sysdba;
drop tablespace TBS_CRUDAPI;
create tablespace TBS_CRUDAPI datafile '/opt/oracle/oradata/FREE/TBS_CRUDAPI.dbf' size 2048M;
drop user c##crudapi;
create user c##crudapi identified by crudapi;
alter user c##crudapi default tablespace TBS_CRUDAPI;
grant connect, resource, dba, create session to c##crudapi;
SELECT USERNAME,DEFAULT_TABLESPACE FROM DBA_USERS;
```