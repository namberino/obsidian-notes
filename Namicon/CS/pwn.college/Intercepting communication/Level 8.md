This level requires us to manually send an Ethernet packet with the type `0xFFFF`, which is not really a valid [EtherType](https://en.wikipedia.org/wiki/EtherType), but oh well. Since we don't really know the MAC address of the remote host, we'll broadcast our packet to every host on the network.

We'll need to first get our MAC address. We can do that by using `ifconfig`. Then we can use `scapy`, which is a networking library in Python, to create our Ethernet packet and broadcast it via our `eth0` interface.

```python
from scapy.all import *

eth = Ether(src="c2:dd:1b:7a:3d:e0", type=0xFFFF)
sendp(eth, iface="eth0")
```