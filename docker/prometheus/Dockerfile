# docker/prometheus/Dockerfile
FROM ubuntu:22.04

# Install Prometheus
RUN apt-get update && apt-get install -y wget tar && \
    wget https://github.com/prometheus/prometheus/releases/download/v2.51.1/prometheus-2.51.1.linux-amd64.tar.gz && \
    tar -xvf prometheus-2.51.1.linux-amd64.tar.gz && \
    mv prometheus-2.51.1.linux-amd64 /opt/prometheus && \
    rm prometheus-2.51.1.linux-amd64.tar.gz

# Copy your config file to the correct path
COPY prometheus.yml /opt/prometheus/prometheus.yml

EXPOSE 9090

# Use the config from the correct location
CMD ["/opt/prometheus/prometheus", "--config.file=/opt/prometheus/prometheus.yml"]
