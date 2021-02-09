FROM alpine:3.12 as certs
RUN apk --update add ca-certificates

FROM alpine:3.12 as otelcol
ADD https://github.com/open-telemetry/opentelemetry-collector-contrib/releases/download/v0.19.0/otelcontribcol_linux_amd64 /otelcol
RUN chmod 755 /otelcol

FROM openjdk:11.0.10-jre-slim-buster
ADD https://oss.jfrog.org/artifactory/oss-snapshot-local/io/opentelemetry/contrib/opentelemetry-java-contrib-jmx-metrics/0.0.1-SNAPSHOT/opentelemetry-java-contrib-jmx-metrics-0.0.1-20210122.210126-6.jar /jmx-metrics.jar
COPY --from=certs /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
COPY --from=otelcol /otelcol /
COPY collector-config.yml /etc/otel/config.yaml

ENTRYPOINT ["/otelcol"]
CMD ["--config", "/etc/otel/config.yaml"]
EXPOSE 55678 55679