## Policy Rules Syntax

Wag allows you to define policies based on port, route and protocol. The following are examples of these definitions.


### Any 

When no other rules are defined or the `any` keyword is used wag will allow all services and port combinations.

Example: 

```sh
"1.1.1.1 54/any": Allows both tcp and udp to 1.1.1.1/32
"1.1.1.1":        Allows all ports and protocols to 1.1.1.1/32
```

### Single Service

Example:
```sh
"192.168.1.1 22/tcp 53/udp": Fairly self explanatory, allows you to hit 22/tcp and 53/udp on a host
"1.1.1.1 icmp":              As icmp doesnt have ports really you dont need it either
```

### Ranges
You can also define a range of ports with a protocol. wag requires that the lower port is first. 

Example:
```sh
"192.168.1.1 22-1024/tcp 53-23/any": Format is low port-high port/service
```