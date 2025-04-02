# Installing Prometheus & Grafana with Docker
Prometheus is a free tool that helps you monitor and alert you about your systems.
Grafana is an open-source platform that helps you visualize and understand your data. 
We will integrate prometheus with grafana, which will be used as a data source for visualization

I will be installing both of them on a raspberry pi with Docker.

Creating an internal network for docker, which will be used to send information between the containers.

```sh
docker network create monitoring
```

### Dockerfile

```yaml
services:
  prometheus:
     image: prom/prometheus
     volumes:
       - /opt/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
     networks:
       - monitoring

   grafana:
     image: grafana/grafana
     networks:
       - monitoring

   networks:
     monitoring:
       external: true
```