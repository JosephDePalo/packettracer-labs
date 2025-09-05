# Creating VLANs

The objective of this lab is to segment a switched network using VLAN tags.

There are two departments, `Sales` and `Eng`, that are on the same subnet.
These two departments should not be able to communicate with each other and
broadcast traffic for either department should only reach computers that
belong to that department.

## Connectivity Before Configuration

- Open the command prompt on `Eng1`.
- Execute `ping 10.10.255.255`, the broadcast address of the network. Observe
  that there are replies from 3 different addresses belonging to
  `Sales1`, `Sales2`, and `Eng2`.

## Configuration

### SW1 Configuration

- Enter privileged exec mode and execute `show vlan`. Observe that all of the
  interfaces are in the default VLAN, VLAN 1.
- Enter configuration mode. Create VLAN 10 and name it `Eng`. Create VLAN 20
  and name it `Sales`.
- Enter configuration mode. Configure the connected interfaces such that
  traffic coming from `Eng` computers are tagged with the `Eng` VLAN and
  traffic coming from `Sales` computers are tagged with the `Sales` VLAN.

#### SW1 Commands

```ios
SW1>enable
SW1#configure terminal
SW1(config)#vlan 10
SW1(config-vlan)#name Eng
SW1(config)#vlan 20
SW1(config-vlan)#name Sales
SW1(config)#interface fa0/1
SW1(config-if)# switch access vlan 10
SW1(config)#interface fa0/2
SW1(config-if)# switch access vlan 20
SW1(config)#interface fa0/3
SW1(config-if)# switch access vlan 10
SW1(config)#interface fa0/4
SW1(config-if)# switch access vlan 20
```

## Connectivity After Configuration

- Open the command prompt on `Eng1`.
- Execute `ping 10.10.255.255` and observe that only `Eng2` replies.
- Execute `ping 10.10.20.10`. Observe that there is no reply.
- Open the command prompt on `Sales1`.
- Execute `ping 10.10.255.255` and observe that only `Sales2` replies.
- Execute `ping 10.10.10.10`. Observe that there is no reply.
