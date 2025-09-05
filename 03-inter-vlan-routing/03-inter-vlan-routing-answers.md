# Inter-VLAN Routing

The objective of this lab is to configure inter-VLAN routing, which will allow
different VLANs to communicate on layer 3.

> **NOTE:** This lab is built off of `2-creating-vlans`, so it is recommended
> to complete that before starting this one.

Some things to note:

- All `Eng` computers are part of the `10.10.10.0/24` subnet and on VLAN 10.
- All `Sales` computers are part of the `10.10.20.0/24` subnet and on VLAN 20.
- All `Eng` computers have a default gateway of `10.10.10.1`.
- All `Sales` computers have a default gateway of `10.10.20.1`.

## Separate Interfaces

- On `R1`, configure `Gig0/0/0` to be the default gateway for the `Eng`
  computers and enable it.
- Configure `Gig0/0/1` to be the default gateway for the `Sales` computers and
  enable it.
- Open the command prompt on `Eng1` and ping `Sales1`. You should get replies.
  On `Sales1`, ping `Eng1`.

### Separate Interfaces Commands

```ios
R1(config)#interface gig0/0/0
R1(config-if)#ip address 10.10.10.1 255.255.255.0
R1(config-if)#no shut
R1(config)#interface gig0/0/1
R1(config-if)#ip address 10.10.20.1 255.255.255.0
R1(config-if)#no shut
```

## Router on a Stick (ROAS)

If you use a router interface for every VLAN on the network, you'll run out of
interfaces very quickly. Having each VLAN use a separate interface has its
perks, but it scales poorly. **Router on a Stick (ROAS)** is a different way to
perform inter-VLAN routing that uses a subinterface for each VLAN.

- On `R1`, shutdown `Gig0/0/1`. Remove the IP address from `Gig0/0/0` as well.
- Configure `Gig0/0/0.10` to belong to the `Eng` VLAN and make it the default
  gateway for the `Eng` computers.
- Configure `Gig0/0/0.20` to belong to the `Sales` VLAN and make it the default
  gateway for the `Sales` computers.
- On `SW1`, enter privileged exec mode and run `show vlan`. Note that `Fa0/5`
  is in VLAN 1. This means that all traffic coming from `R1` will be tagged as
  VLAN 1 traffic and won't be forwarded to VLAN 10 or VLAN 20 devices.
  Configure `Fa0/5` as a trunk port.
- Open the command prompt on `Eng1` and ping `Sales1`. You should get replies.
  On `Sales1`, ping `Eng1`.

### ROAS Commands

```ios
R1(config)#interface gig0/0/1
R1(config-if)#shutdown
R1(config)#interface gig0/0/0
R1(config-if)#no ip address 10.10.10.1 255.255.255.0
R1(config)#interface gig0/0/0.10
R1(config-subif)#encapsulation dot1q 10
R1(config-subif)#ip address 10.10.10.1 255.255.255.0
R1(config)#interface gig0/0/0.20
R1(config-subif)#encapsulation dot1q 20
R1(config-subif)#ip address 10.10.20.1 255.255.255.0

SW1(config)#interface fa0/5
SW1(config-if)#switchport mode trunk
```

## Layer 3 Switches

Switches with routing capabilities can perform inter-VLAN routing using
**switched virtual interfaces (SVIs)**.

- On `R1`, shutdown `Gig0/0/0`.
- On `SW1`, enable IP routing.
- Configure the VLAN 10 interface as the default gateway for the `Eng`
  computers.
- Configure the VLAN 20 interface as the default gateway for the `Sales`
  computers.
- Open the command prompt on `Eng1` and ping `Sales1`. You should get replies.
  On `Sales1`, ping `Eng1`.

### Layer 3 Switch Commands

```ios
R1(config)#interface gig0/0/0
R1(config-if)#shutdown

SW1(config)#ip routing
SW1(config)#interface vlan 10
SW1(config-if)#ip address 10.10.10.1 255.255.255.0
SW1(config)#interface vlan 20
SW1(config-if)#ip address 10.10.20.1 255.255.255.0
```
