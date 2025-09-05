# Basic Setup

The objective of this lab is to configure a simple network that will allow a PC
to reach the "internet." This lab will cover:

- Setting device host names.
- Statically configuring IPv4 addresses on physical interfaces.
- Using the ping utility in Packet Tracer to test connectivity.

## Configuration

### PC1

- Go to the `Config` menu and select `FastEthernet0`. Set an IPv4 address of
  `10.10.10.2` and subnet mask of `255.0.0.0`.
- Go to the `Settings` menu and set the default IPv4 gateway to `10.10.10.1`.

### SW1

- Enter privileged exec mode and then configuration mode.
- Set the host name to `SW1`.

No additional configuration is needed. The interfaces of switches are
automatically up and will be able to transmit frames from `PC1` to `R1`.

### R1

- Enter privileged exec mode and then configuration mode.
- Set the host name to `R1`.
- Hover over the red triangle left of `R1`. That is the interface the router is
  connected to `SW1` on, `Gig0/0` or `GigabitEthernet0/0`.
- Enter the configuration for `Gig0/0`. Configure it to have an IPv4 address of
  `10.10.10.1` and a subnet mask of `255.0.0.0`.
- Bring `Gig0/0` up. The red triangle for the `Gig0/0` interface should change
  into a green circle.

## Testing Connectivity

- Open the `Command Prompt` from the `Desktop` menu on `PC1`.
- Execute `ping 203.0.113.1`. The command should be successful.
- Execute `tracert 203.0.113.1`. You should see the traffic first go to
  `10.10.10.1` before going to `203.0.113.1`.
