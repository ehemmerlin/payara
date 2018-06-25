FROM payara/server-full:4.181
USER root
RUN \
  apt-get update \
  && apt-get -y install gettext-base \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

COPY bankFacade-ejb-client.jar /opt/payara41/glassfish/domains/domain1/autodeploy/
COPY bankFacade-ejb.jar /opt/payara41/glassfish/domains/domain1/autodeploy/
COPY bankFacade-war.war /opt/payara41/glassfish/domains/domain1/autodeploy/
COPY webStore.war /opt/payara41/glassfish/domains/domain1/autodeploy/

USER payara
RUN curl -o /opt/payara41/glassfish/domains/domain1/lib/ext/mysql-connector-java-5.1.46.jar https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.46/mysql-connector-java-5.1.46.jar
RUN cp /opt/payara41/glassfish/domains/domain1/lib/ext/mysql-connector-java-5.1.46.jar /opt/payara41/glassfish/lib/

ENTRYPOINT ${PAYARA_PATH}/generate_deploy_commands.sh && \
echo "create-jdbc-connection-pool --datasourceclassname com.mysql.jdbc.jdbc2.optional.MysqlDataSource --restype javax.sql.DataSource --property User=${MYSQL_USER}:Port=${MYSQL_PORT}:Password=${MYSQL_PASSWORD}:Url='jdbc\:mysql\://${MYSQL_HOST}\:${MYSQL_PORT}\/${MYSQL_DATABASE}' mysql" > mycommands.asadmin && \
echo "create-jdbc-resource --connectionpoolid mysql ${JDBC_RESOURCE_NAME}" >> mycommands.asadmin && \
echo "create-jms-resource --restype javax.jms.Queue --property Name=PhysicalQueue ${JMS_RESOURCE_NAME}" >> mycommands.asadmin && \
cat ${DEPLOY_COMMANDS} >> mycommands.asadmin && \
${PAYARA_PATH}/bin/asadmin start-domain -v --postbootcommandfile mycommands.asadmin ${PAYARA_DOMAIN}