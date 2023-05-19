ARG DEBEZIUM_VERSION
FROM quay.io/debezium/connect:${DEBEZIUM_VERSION}

ARG JMX_AGENT_VERSION
RUN mkdir /kafka/etc && cd /kafka/etc &&\
        curl -so jmx_prometheus_javaagent.jar \
        https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/$JMX_AGENT_VERSION/jmx_prometheus_javaagent-$JMX_AGENT_VERSION.jar

COPY config.yml /kafka/etc/config.yml

COPY kafka-connect-jdbc-10.6.0.jar /kafka/connect/kafka-connect-jdbc/kafka-connect-jdbc-10.6.0.jar

COPY mysql-connector-j-8.0.31.jar /kafka/libs/mysql-connector-j-8.0.31.jar

COPY postgresql-42.3.7.jar /kafka/libs/postgresql-42.3.7.jar
