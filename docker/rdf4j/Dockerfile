FROM docker.io/tomcat:7.0.96

MAINTAINER Ankita Likhyani

COPY . /usr/local/tomcat/

ARG MAVEN_VERSION=3.6.3

ARG USER_HOME_DIR="/root"

RUN mkdir -p /usr/share/maven && \
curl -fsSL https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/$MAVEN_VERSION/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar -xzC /usr/share/maven --strip-components=1 && \
ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

ENV MAVEN_HOME /usr/share/maven

ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

ENV MAVEN_OPTS="-XX:+TieredCompilation -XX:TieredStopAtLevel=1"

RUN mvn clean install -Dmaven.test.skip=true

RUN cp -pr  /usr/local/tomcat/RDF4J/rdf4j-workbench/target/rdf4j-workbench-0.0.1-SNAPSHOT.war /usr/local/tomcat/webapps/rdf4j-workbench.war

RUN cp -rp /usr/local/tomcat/RDF4J/rdf4j-server/target/rdf4j-server-0.0.1-SNAPSHOT.war /usr/local/tomcat/webapps/rdf4j-server.war
EXPOSE 8080

# to build this container in podman use the following command from the root directory of the project
# podman build -t rdf4j_ergo -f docker/rdf4j/Dockerfile .
