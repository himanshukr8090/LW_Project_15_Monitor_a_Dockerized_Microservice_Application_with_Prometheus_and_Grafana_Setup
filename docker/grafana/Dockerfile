FROM ubuntu:22.04
RUN apt update && apt install -y wget software-properties-common apt-transport-https gnupg2 && \
    wget -q -O - https://packages.grafana.com/gpg.key | apt-key add - && \
    add-apt-repository "deb https://packages.grafana.com/oss/deb stable main" && \
    apt update && apt install -y grafana
EXPOSE 3000
CMD ["/usr/sbin/grafana-server", "--homepath=/usr/share/grafana"]
