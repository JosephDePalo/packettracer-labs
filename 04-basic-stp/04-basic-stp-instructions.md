# Basic STP

The objective of this lab is to go over basic STP configuration and
examination.

## Spanning Tree Before Configuration

- Using the link lights and `show spanning-tree`, determine which switch is the
  root bridge.
- Why is `SW1` the root bridge?
- Which ports are root, designated, or alternative?
- Which mode is being used for STP?
- From `PC1`, ping `PC2`. Note down the MAC address of `PC2` and then go to
  each switch and run the `show mac-address-table` command to see the path the
  ICMP traffic took.

## Configuration

- Change the STP mode to RPVST+ on all switches.
- Change the priority on `SW2` such that it becomes the root bridge. Observe
  as `SW2` becomes the root switch almost instantly due to RPVST+.
- Set root guard on the `Fa0/1` interface of `SW2`.
- On the `Fa0/3` interfaces of `SW2` and `SW3`, enable portfast. These
  interfaces connect to endpoints, so we do not need to worry about loops
  forming from here.

## Spanning Tree After Configuration

- From `PC1`, ping `PC2`. Note down the MAC address of `PC2` and then go to
  each switch and run the `show mac-address-table` command to see the path the
  ICMP traffic took.
- On `SW2` shutdown the `Fa0/3` interface and then bring it back up.
  Immediately run `show spanning-tree` and note that the interface is instantly
  put into a forwarding state.
