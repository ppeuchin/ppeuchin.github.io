---
title: Palo Alto Tap Mode Deployment
description: Deploying Palo Alto Firewall in tap mode with a Cisco switch configured as the SPAN switch.
date: 2024-02-16 00:00:00 +0000
categories: [firewalls]
tags: [networking, palo alto, security]
toc: true
---

Tap mode deployment allows you to passively monitor traffic flows across a network by way of a switch SPAN or mirror port. The SPAN port copies traffic from other ports on the switch and forwards it out the dedicated destination SPAN port for analysis by a network analyzer tool like Wireshark.

By deploying the firewall in tap mode, you can get visibility into what applications are running on your network without having to make any changes to your network design. In addition, when in tap mode, the firewall can identify threats on your network, but because the traffic is not running through the firewall while it is in tap mode, it cannot take any action on the traffic, such as blocking traffic with threats or applying QoS traffic control.

## Demo

This demo includes the use of a cisco switch as the SPAN configured switch and the Palo Alto firewall in tap mode.

![demo-image](/assets/img/palo-alto-tap-mode-deployment/demo-image.png)

### Configuring SPAN switch

We’ll create a SPAN session and specify the source and destination ports.

We’ll be copying traffic sent from and received on ports Gi0/0 and Gi0/1 on the switch. Those are the source ports. 

The destination port Gi0/2 is the port that the copied traffic will be sent through to the network analyzer, in our case, the Palo Alto firewall deployed in tap mode.

```bash
Switch(config)# monitor session 1 source interface g0/0 - 1 both
Switch(config)# monitor session 1 destination interface g0/2
```

`both` here means that we want to monitor traffic that are being transmitted and received on the specified ports.

### Configuring Tap mode

First, we create a tap zone

![zone](/assets/img/palo-alto-tap-mode-deployment/zone-image.png)

Then, we configure the interface (Gi0/2) connected to the SPAN configured switch as a tap interface and assign it to the tap zone

![tapint](/assets/img/palo-alto-tap-mode-deployment/tapint-image.png)

Now, we create a security profile group. Security profile groups allows to specify sets of security profiles as a unit and then added to security policies.

![secprof](/assets/img/palo-alto-tap-mode-deployment/secprof-image.png)

And create a security policy named TAP. It can be anything you want.

![secpol](/assets/img/palo-alto-tap-mode-deployment/secpol-image.png)

The source and destination zones should be the tap zone we created earlier.

![polzones](/assets/img/palo-alto-tap-mode-deployment/polzones.png)
_Source Zone_

![polzones](/assets/img/palo-alto-tap-mode-deployment/polzone.png)
_Destination Zone_

We set the profile setting to group and select the group profile we created earlier. And check log at session start as well.

![polrule](/assets/img/palo-alto-tap-mode-deployment/polrule.png)

Now, we’re all set. Commit the changes, go to the monitor tab then session browser and refresh. You should see the traffic being sent to/from the devices connected to the monitored ports.

## Helpful Resources

Palo Alto Docs: [Tap Interfaces (paloaltonetworks.com)](https://docs.paloaltonetworks.com/pan-os/10-1/pan-os-networking-admin/configure-interfaces/tap-interfaces)

Configuring SPAN CISCO PDF: [swspan.pdf (cisco.com)](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst2960/software/release/12-2_40_se/configuration/guide/scg/swspan.pdf)

Juniper: [Configuring Port Mirroring: EX Series and QFX Series (youtube.com)](https://www.youtube.com/watch?v=dS04eTQLk_A)