# oracle

# build
```bash
git clone git@github.com:oracle/docker-images.git
cd docker-images/OracleDatabase/SingleInstance/dockerfiles
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
    -e ORACLE_EDITION=enterprise \
    -e ORACLE_CHARACTERSET=AL32UTF8 \
    -e ENABLE_ARCHIVELOG=true \
    -v /opt/crudapi/oracle:/opt/oracle/oradata \
    -d oracle:21.3.0

docker logs -f oracle
``` 

## docs
[Oracle Database container images](https://github.com/oracle/docker-images/blob/main/OracleDatabase/SingleInstance/README.md)

