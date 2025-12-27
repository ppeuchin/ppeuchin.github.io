---
title: Aruba CX Initial Configuration
date: 2024-06-09 00:00:00 +0000
categories: [aruba]
tags: [networking]     # TAG names should always be lowercase
# description: Short summary of the post.
toc: true
---

Aruba CX switches have four contexts for managing the device; Operational, Manager, Global, and Interface contexts. A context is like a scope or an environment within the device, where you can execute operational or managerial commands specific to that scope or environment.

When you first log into the switch, you see this. 

```bash
switch>
```

This is the operational context (denoted by `>`). It allows you to only execute a limited number of operational commands.

The next context is the Manager context and we get into it by executing the `enable` command. It is denoted by `#`

```bash
switch> enable
switch#
```

From here, we can configure the device’s hostname by getting into the Global context. We do that by executing `configure terminal` or `conf t` for short, the switch will know what that means. Then we use the `hostname` command to configure the device’s hostname.

```bash
switch# configure terminal
switch(config)# hostname ArubaCX
ArubaCX(config)#
```

Now, we’d like to get into the interface context and configure a few interfaces for connectivity. By default, the interfaces are routed interfaces. We execute the `interface <interface-id>` command to configure a specific interface, in our case,  `interface 1/1/1`.  Interface context is denoted by `(config-if)` for configure interface.

```bash
# configuring an ip on a routed/layer 3 port
ArubaCX(config)# interface 1/1/1
ArubaCX(config-if)# ip address 10.0.0.1/24
ArubaCX(config-if)# no shutdown

# configuring a switchport/layer 2 port
ArubaCX(config)# interface 1/1/2
ArubaCX(config-if)# no routing
ArubaCX(config-if)# vlan access 2
ArubaCX(config-if)# no shutdown
```

>The `no shutdown` command is to enable the interface for communication.

>**Note:** You can only get into the interface context from the global context, global from manager, and manager from operational, following the hierarchy (Operational > Manager > Global > Interface). You can go back to Global from Interface using `exit`, and to Manager from Interface using `end`.
{: .prompt-info}

```bash
# from interface context to global context
ArubaCX(config-if)# exit
ArubaCX(config)# 

# from interface context to manager context
ArubaCX(config-if)# end
ArubaCX#
```

Any new config change, followed by a 5 minute idle period, will create a new checkpoint that can later be used to back up or restore the running config state.

> On-demand checkpoints can be created by saving the running config or creating a custom checkpoint.
{: .prompt-info}

