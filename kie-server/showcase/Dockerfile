#################################################################################
# Dockerfile that provides the image for JBoss KIE Server 7.25.0.Final Showcase
#################################################################################

####### BASE ############
FROM jboss/kie-server:7.31.0.Final

####### MAINTAINER ############
MAINTAINER "Neil Chen" "chenjihu@gmail.com"

####### ENVIRONMENT ############
# ENV KIE_SERVER_ID kie-server
# ENV KIE_SERVER_LOCATION http://localhost:8080/kie-server/services/rest/server
ENV KIE_SERVER_USER kieserver
ENV KIE_SERVER_PWD kieserver1!
# When using this image against jboss/jbpm-workbench-showcase use /jbpm-console/rest/controller
# ENV KIE_SERVER_CONTROLLER http://localhost:8080/kie-wb/rest/controller

#定义 kie-workbench的服务名，用于运行在k8s中
ENV KIE_WB_PORT_8080_TCP_ADDR kie-wb

#Workbench的工作url路径，一般不会变
ENV KIE_WB_ENV_KIE_CONTEXT_PATH business-central

#这里需要用到workbench的登录用户名和密码
ENV KIE_SERVER_CONTROLLER_USER kie
ENV KIE_SERVER_CONTROLLER_PWD IamKie2019clp
#ENV KIE_MAVEN_REPO http://kie-wb:8080/business-central/maven2
ENV KIE_MAVEN_REPO_USER kie
ENV KIE_MAVEN_REPO_PASSWORD IamKie2019clp
ENV JAVA_OPTS -Xms300m -Xmx2048m -Djava.net.preferIPv4Stack=true -Dfile.encoding=UTF-8

####### Drools KIE Server CUSTOM CONFIGURATION ############
RUN mkdir -p $HOME/.m2
ADD etc/standalone-full-kie-server.xml $JBOSS_HOME/standalone/configuration/standalone-full-kie-server.xml
ADD etc/kie-server-users.properties $JBOSS_HOME/standalone/configuration/kie-server-users.properties
ADD etc/kie-server-roles.properties $JBOSS_HOME/standalone/configuration/kie-server-roles.properties
ADD etc/start_kie-server.sh $JBOSS_HOME/bin/start_kie-server.sh
ADD etc/settings.xml $JBOSS_HOME/../.m2/settings.xml

# Added files are chowned to root user, change it to the jboss one.
USER root
RUN chown jboss:jboss $JBOSS_HOME/standalone/configuration/standalone-full-kie-server.xml && \
chown jboss:jboss $JBOSS_HOME/standalone/configuration/kie-server-users.properties && \
chown jboss:jboss $JBOSS_HOME/standalone/configuration/kie-server-roles.properties && \
chown jboss:jboss $JBOSS_HOME/bin/start_kie-server.sh && \
chown jboss:jboss $JBOSS_HOME/../.m2/settings.xml

####### JBOSS USER ############
# Switchback to jboss user
USER jboss

####### RUNNING KIE-SERVER ############
WORKDIR $JBOSS_HOME/bin/
CMD ["./start_kie-server.sh"]
