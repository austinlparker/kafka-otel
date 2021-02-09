FROM maven:3.6.0-jdk-11-slim AS build
COPY src /home/app/src
COPY pom.xml /home/app
RUN mvn -f /home/app/pom.xml clean package

FROM openjdk:11-jre-slim
COPY --from=build /home/app/target/otel-spring-0.0.1-SNAPSHOT.jar /usr/local/lib/otelspring.jar
ADD https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent-all.jar /usr/local/lib/otel.jar
EXPOSE 9000
ENTRYPOINT ["java","-javaagent:/usr/local/lib/otel.jar","-jar","/usr/local/lib/otelspring.jar"]