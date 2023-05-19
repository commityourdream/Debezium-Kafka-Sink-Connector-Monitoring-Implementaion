# Latest-Sink-Connector-Setup


This setup is going to demonstrate how to receive events from MySQL database and stream them down to a PostgreSQL database using Kafka, Debezium, JDBC and Grafana.




                   +-------------+
                   |             |
                   |    MySQL    |
                   |             |
                   +------+------+
                          |
                          |
                          |
          +---------------v------------------+
          |                                  |
          |           Kafka Connect          |
          |  (Debezium, JDBC connectors)     |
          |                                  |
          +---------------+------------------+
                          |
                          |
                          |
                          |
                  +-------v--------+
                  |                |
                  |   PostgreSQL   |
                  |                |
                  +----------------+


# Steps To Follow:

Start the application:
```
export DEBEZIUM_VERSION=2.0
```
Up the Docker:

```
docker compose -f docker-compose.yml up --build
```

Start MySQL connector
```
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @mysql-source-connector.json
```
Start PostgreSQL connector
```
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @postgres-sink-connector.json
```
Check contents of the MySQL database:
```
docker compose -f docker-compose.yml exec mysql bash -c 'mysql -u $MYSQL_USER -p$MYSQL_PASSWORD inventory -e "select * from customers"'
```
Verify that the PostgreSQL database has the same content:
```
docker compose -f docker-compose.yml exec postgres bash -c 'psql -U $POSTGRES_USER $POSTGRES_DB -c "select * from customers"'
```

Insert a new record into MySQL;
```
docker compose -f docker-compose.yml exec mysql bash -c 'mysql -u $MYSQL_USER  -p$MYSQL_PASSWORD inventory'
```
```
insert into customers values(default, 'Amit', 'Manjhi', 'a.manjhi@example.com');
```

Verify that PostgreSQL contains the new record:
```
docker compose -f docker-compose-jdbc.yaml exec postgres bash -c 'psql -U $POSTGRES_USER $POSTGRES_DB -c "select * from customers"'
```
Grafana DashBoard
<img width="960" alt="source-sink-dashboard" src="https://github.com/commityourdream/Latest-Sink-Connector-Setup/assets/42512407/2bd63d77-8d17-4d0f-a0fa-8f5de1a7047c">
