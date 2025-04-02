# Configuring Cisco Switches for Prometheus

## Introduction
I have two cisco switches which i would be adding to prometheus; Catalyst 2960 and 3550. I will configure snmp on those switches. 

## Enable SNMP on the Cisco Switches
<b>Catalyst 3550 configuration</b>
```
interface FastEthernet 0/1
  ip address 192.168.0.99 255.255.255.0
  no switchport
!
ip default-gateway 192.168.0.1
snmp-server community public RO
```
<b>Catalyst 2960 configuration</b>
```
interface Vlan1
  ip address 192.168.0.59 255.255.255.0
!
ip default-gateway 192.168.0.1
snmp-server community public RO
```

## Install SNMP Exporter
```yaml
snmp-exporter:
  image: prom/snmp-exporter
  container_name: snmp_exporter
  volumes:
    - /opt/snmp_exporter/snmp.yml:/etc/snmp_exporter/snmp.yml
  ports:
    - 9116:9116
  networks:
    - monitoring
```

## Configure Prometheus to Scrape the Switches
In the Prometheus configuration file (`prometheus.yml`).
```yaml
scrape_configs:
  - job_name: 'snmp'
    metrics_path: /metrics
      static_configs:
        - targets:
            - 192.168.0.99
            - 192.168.0.57
      relabel_configs:
        - source_labels: [__address__]
          target_label: [__param_target]
        - source_labels: [__param_target]
          target_label: instance
        - target_label: __address__
          replacement: snmp-exporter:9116

  - job_name: 'snmp-exporter'
      static_configs:
        - targets: ['snmp-exporter:9116']
```

<b>Restarting Prometheus to apply the changes</b>
```
docker restart <container-name/id>
```
