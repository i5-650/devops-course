FROM postgres:14.1-alpine

# copy script to create db
COPY ./sql-scripts/* /docker-entrypoint-initdb.d
