FROM ubuntu:22.04
RUN apt update && apt install -y wget tar && \
    wget https://github.com/prometheus/node_exporter/releases/download/v1.8.1/node_exporter-1.8.1.linux-amd64.tar.gz && \
    tar -xvf node_exporter-*.tar.gz && mv node_exporter-1.8.1.linux-amd64 /opt/node_exporter
EXPOSE 9100
CMD ["/opt/node_exporter/node_exporter"]
