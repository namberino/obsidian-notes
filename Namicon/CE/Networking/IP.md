# IPv4 (32 bits)
## Subnets (individual networks)
An IP address with 24 network bits and 8 host bits: $198.51.100.0/24$

```
        24 network bits        | 8 host bits
-------------------------------+---------
11000110 . 00110011 . 01100100 . 00000000
  198         51        100          0
```

Example IPs on the subnet:
```
198.51.100.2
198.51.100.3
198.51.100.4
198.51.100.30
198.51.100.212
```

Any number of bits can be in the allocated for the network bits at the cost of host bits

Subnets can have subnets, at the cost of host bits

> First and last address will always be occupied

## Subnet masks
A number that when ANDed with any IP address, will give you the subnet number
```
        24 network bits          | 8 host bits
  -------------------------------+---------
  11000110 . 00110011 . 01100100 . 01000011      198. 51.100.67
& 11111111 . 11111111 . 11111111 . 00000000    & 255.255.255. 0
-------------------------------------------    ----------------
  11000110 . 00110011 . 01100100 . 00000000      198. 51.100. 0
```

## Special addresses
- 127.0.0.1 - this is the computer you are on now. It’s often mapped to the name `localhost`.
- 0.0.0.0 - Reserved. Host 0 on any subnet is reserved.
- 255.255.255.255 - Broadcast. Intended for all hosts on a subnet. Though it seems like this would broadcast to the entire Internet, routers don’t forward packets intended for this address

You can also broadcast to your local subnet by sending to the host with all bits set to 1. For example, the subnet broadcast address for 198.51.100.0/24 is 198.51.100.255.

## Special subnets
There are some reserved subnets you might come across:
- 10.0.0.0/8 - Private network addresses (very common)
- 127.0.0.0/8 - This computer, via the loopback device.
- 172.16.0.0/12 - Private network addresses
- 192.0.0.0/24 - Private network addresses
- 192.0.2.0/24 - Documentation
- 192.168.0.0/16 - Private network addresses (very common)
- 198.18.0.0/15 - Private network addresses
- 198.51.100.0/24 - Documentation
- 203.0.113.0/24 - Documentation
- 233.252.0.0/24 - Documentation

You’ll find your home IPs are in one of the “Private” address ranges. Probably 192.168.x.x.

Any documentation that you write that requires example (not real) IP addresses should use any of the ones marked “Documentation”, above.

# IPv6
## Presentation
A 64 network bit and 64 host bit IP (using slash notation): 
```
2001:0db8:6ffa:8939:163b:4cab:98bf:070a/64
```

## Special addresses and subnets
- ::1 - localhost, this computer, IPv6 version of 127.0.0.1
- 2001:db8::/32 - for use in documentation
- fe80::/10 - link local address

## DNS
