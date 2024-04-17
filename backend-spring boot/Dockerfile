# Stage 1: Build stage
FROM maven:3.8.4-openjdk-17 AS MAVEN_BUILD

# Copy the pom.xml and Maven wrapper script (if any)
COPY pom.xml /build/
COPY mvnw /build/
# Copy the source code
COPY src /build/src/

# Set working directory
WORKDIR /build/

# Run Maven build
RUN mvn clean install -Dmaven.test.skip=true -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=WARN -B --no-transfer-progress -e

# Stage 2: Production stage
FROM openjdk:17-oracle

# Set working directory
WORKDIR /app

# Copy the generated JAR file from the build stage to the production image
COPY --from=MAVEN_BUILD /build/target/Zosh-Food.jar /app/flow-api.jar

# Expose port (if needed)
EXPOSE 5000

# Set the entry point for the application
ENTRYPOINT ["java", \
            "-Dfile.encoding=UTF-8", \
            "-Djava.security.egd=file:/dev/./urandom", \
            "-jar", \
            "/app/flow-api.jar" \
           ]
