# Basic STP

The objective of this lab is to go over basic STP configuration and
examination.

## Spanning Tree Before Configuration

- Using the link lights and `show spanning-tree`, determine which switch is the
  root bridge.
- Why is `SW1` the root bridge?
  - **The priority of all of the switches are equal, so the switch with the
    lowest MAC address (`SW1`) will become the root bridge.**
- Which ports are root, designated, or alternative?
  - **On `SW1`, `Fa0/1` and `Fa0/2` are designated ports.**
  - **On `SW2`, `Fa0/1` is a root port and `Fa0/2` is an alternative port.**
  - **On `SW3`, `Fa0/1` is a designated port and `Fa0/2` is a root port.**
  - **All interfaces connected to PCs are designated ports.**
- Which mode is being used for STP?
  - **`show spanning-tree summary` indicates PVST is being used.**
- From `PC1`, ping `PC2`. Note down the MAC address of `PC2` and then go to
  each switch and run the `show mac-address-table` command to see the path the
  ICMP traffic took.
  - **Traffic goes from `PC1->SW2->SW1->SW3->PC2`.**

## Configuration

- Change the STP mode to RPVST+ on all switches.
- Change the priority on `SW2` such that it becomes the root bridge. Observe
  as `SW2` becomes the root switch almost instantly due to RPVST+.
- Set root guard on the `Fa0/1` interface of `SW2`.
- On the `Fa0/3` interfaces of `SW2` and `SW3`, enable portfast. These
  interfaces connect to endpoints, so we do not need to worry about loops
  forming from here.

### Configuration Commands

```ios
SW1(config)#spanning-tree mode rpvst

SW3(config)#spanning-tree mode rpvst
SW3(config)#interface fa0/3
SW3(config-if)#spanning-tree portfast

SW2(config)#spanning-tree mode rpvst
SW2(config)#spanning-tree vlan 1 root primary
SW2(config)#interface fa0/1
SW2(config-if)#spanning-tree guard root
SW2(config)#interface fa0/3
SW2(config-if)#spanning-tree portfast
```

## Spanning Tree After Configuration

- From `PC1`, ping `PC2`. Note down the MAC address of `PC2` and then go to
  each switch and run the `show mac-address-table` command to see the path the
  ICMP traffic took.
  - **Traffic goes from `PC1->SW2->SW3->PC2`.**
- On `SW2` shutdown the `Fa0/3` interface and then bring it back up.
  Immediately run `show spanning-tree` and note that the interface is instantly
  put into a forwarding state.
