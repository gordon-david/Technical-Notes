# Maven Commands

mvn archetype:generate

mvn package -> building an executable, requires properties maven.compiler.source, maven.compiler.target

# Simple Dockerfile Example

```docker
FROM sgrio/java-oracle
MAINTAINER David Gordon (d.phi.gordon@gmail.com)
# building the image
RUN apt-get update
RUN apt-get install

# copy maven xml and source files then package
COPY pom.xml /usr/local/service/pom.xml
COPY src /usr/local/service/src
WORKDIR /usr/local/service
RUN mvn package
CMD ["java", "-cp", "target/docker-service-1.0-SNAPSHOT.jar", "org.gordondavid.app"]
```

Options:

- packaging outside the container, then copying the packaged jar into the container
  - skip copying the pom.xml and running any maven specific commands
  - simply copy the jar and use a CMD to run the executable
- use variable based on jar filename
