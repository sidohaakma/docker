FROM tomcat:7.0-jre8-alpine

ENV MOLGENIS_VERSION=5.2.0-SNAPSHOT

# --no-cache allows users to install packages with an index that is updated and used on-the-fly and not cached locally
# This avoids the need to use --update and remove /var/cache/apk/* when done installing packages.
RUN apk --no-cache add openssl \	
	&& wget -O molgenis-app-$MOLGENIS_VERSION.war http://haakma.org/molgenis/molgenis-app-$MOLGENIS_VERSION.war

RUN rm -r $CATALINA_HOME/webapps/ROOT \
    && rm -r $CATALINA_HOME/webapps/docs \
    && rm -r $CATALINA_HOME/webapps/examples \
    && mv molgenis-app-$MOLGENIS_VERSION.war $CATALINA_HOME/webapps/ROOT.war \
	&& echo 'CATALINA_OPTS="-Xmx1g -XX:+UseConcMarkSweepGC -XX:+CMSClassUnloadingEnabled -Dmolgenis.home=/opt/molgenis/"' > $CATALINA_HOME/bin/setenv.sh

CMD ["catalina.sh", "run"]
