# Kafka Monitoring with OpenTelemetry

This is a work-in-progress repository!

## Goals

- Demonstrate using the OpenTelemetry Collector + JMX Metrics Plugin to monitor a Kafka Cluster
- Demonstrate using OpenTelemetry Java Instrumentation to monitor a Kafka Producer/Consumer application

## Usage

0. Add a Lightstep project access token to the `./collector-java/collector-config.yml` file. 
1. Run `docker-compose up -d` to start Kafka, the test application, and the collector.
2. Run `curl -X POST -F 'message=test' http://localhost:9000/kafka/publish` to write a message to Kafka.
3. Run `docker-compose logs java` -- you should see logging output indicating that a message was produced and consumed.

## How It Works

There's a few things at play here:

1. JMX metrics receiver being proxied through the OTel Collector. This extension allows us to scrape MBeans from any JMX server. [You can learn more about it here](https://github.com/open-telemetry/opentelemetry-java-contrib/blob/main/contrib/jmx-metrics/README.md) -- we could use it with any arbitrary JVM process, or with other target systems -- [see this list](https://github.com/open-telemetry/opentelemetry-java-contrib/blob/main/contrib/jmx-metrics/README.md#target-systems).

2. Spring Boot and Kafka Client trace instrumentation from OpenTelemetry Java Auto-Instrumentation, proxied through the OTel Collector. 

## Next Steps

- More interesting consumer/producer code
- Add collector as agent for consumer/producer JMX/JVM metrics

