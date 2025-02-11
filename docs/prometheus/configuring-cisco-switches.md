# Configuring Cisco Switches for Prometheus

## Introduction
I have two cisco switches which i would be adding to prometheus; Catalyst 2560 and 3550. I will configure snmp on those switches. 

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
<b>Catalyst 2560 configuration</b>
```
interface Vlan1
  ip address 10.0.0.252 255.255.255.0
!
ip default-gateway
```

## Configure Prometheus to Scrape the Switches
In the Prometheus configuration file (`prometheus.yml`).
```yaml
scrape_configs:
  - job_name: 'cisco_switch_3550'
      static_configs:
        - targets: ['192.168.0.99:161']
  - job_name: 'cisco_switch_2560'
      static_configs:
        - targets: ['<SWITCH_IP>:161']
```

<b>Restarting Prometheus to apply the changes</b>
```
docker restart <container-name/id>
``` 

4. **Verify the Configuration**
   - Access the Prometheus web interface.
   - Navigate to the "Targets" page and ensure the Cisco switch is listed and being scraped successfully.