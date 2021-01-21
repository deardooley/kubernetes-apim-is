ARG WSO2_SERVER_NAME=wso2am
ARG WSO2_SERVER_VERSION=3.2.0

FROM wso2/wso2am:${WSO2_SERVER_VERSION}

# build arguments for WSO2 product installation
ARG WSO2_SERVER=${WSO2_SERVER_NAME}-${WSO2_SERVER_VERSION}
ARG WSO2_SERVER_HOME=${USER_HOME}/${WSO2_SERVER}
# build arguments for external artifacts
ARG MYSQL_CONNECTOR_VERSION=8.0.17

ADD --chown=wso2carbon:wso2 https://repo1.maven.org/maven2/mysql/mysql-connector-java/${MYSQL_CONNECTOR_VERSION}/mysql-connector-java-${MYSQL_CONNECTOR_VERSION}.jar ${WSO2_SERVER_HOME}/repository/components/dropins/

