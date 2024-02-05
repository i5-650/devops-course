# Commands for bdd docker

## Build
```bash
# simple command to build the docker image located in the current directory
# the -t flag is used to tag the image with a name
docker build -t postgres-bdd-tp1 .
```

## Run
```bash
docker run \
# --net to connect to a network (hehe network > *)
--net=hehe-network \
# --name to give a name to the container, i was inspired
--name=postgres-bdd-tp1 \
# -e to set environment variables but we already did it in the Dockerfile
# -v to mount a volume, here we mount the sql-scripts directory. Make the db persistent
-v ./local-db:/var/lib/postgresql/data \
# -d to run the container in the background
-d \
# the image to run
postgres-bdd-tp1
```


docker run \
--net=hehe-network \
--name=postgres-bdd-tp1 \
-v /sql-scripts:/docker-entrypoint-initdb.d \
-d \
postgres-bdd-tp1

## See database with adminer
```bash
docker run \
-p "8090:8080" \
--net=hehe-network \
--name=adminer \
-d \
adminer
```
