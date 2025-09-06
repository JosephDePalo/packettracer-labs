# Static Routing

The objective of this lab is to create static routes to different subnets that
are not directly connected.

## Before Route Configuration

- From `PC1` ping `10.10.50.1`. Then ping `10.10.50.2`.
- Run `debug ip icmp` on `R2` and then ping `10.10.50.2` again from `PC1`.
  Replies are being sent, but they are not being received by `PC1`.
- Check if `R2` knows how to reach `PC1` by showing the routing table with
  `show ip route`.
- Verify that `PC2` cannot receive echo replies from `10.10.50.1` either.

## Configuring Static Routes

- `R2` does not know how to reach the `10.10.10.0/24` network. Configure a
  static route on `R2` with a destination network of `10.10.10.0/24` and next
  hop address of `10.10.50.1`.
- `R1` does not know how to reach the `10.10.20.0/24` network. Configure a
  static route on `R1` with a destination network of `10.10.20.0/24` and next
  hop address of `10.10.50.2`.

### Configuration Commands

```ios
R2(config)#ip route 10.10.10.0 255.255.255.0 10.10.50.1

R1(config)#ip route 10.10.20.0 255.255.255.0 10.10.50.2
```

## Verify Connectivity

- From `PC1`, ping `PC2`. If replies are received, traffic has successfully
  been routed across all networks.
- Ping `PC2` from `PC1`.
