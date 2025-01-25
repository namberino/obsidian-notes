This level requires us to send an IP packet to a remote host at `10.0.0.3` with `proto` (protocol option) set to `0xFF`, which is a [reserved protocol number](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers). So same thing here, use `scapy` to construct the packet and send it. The `IP` object in `scapy` allows us to do that, except we need a source IP address. Using the `ifconfig`, it shows us the IPv6 of the `eth0` interface. We need an IPv4 version since we're transporting to an IPv4 address. It turns out `scapy` lets us send IP packets using the source interface's MAC address and a destination IP address. 

```python
from scapy.all import *

eth = Ether(src="da:d9:12:2b:10:fa")
ip = IP(dst="10.0.0.3", proto=0xFF)
sendp(eth / ip, iface="eth0")
```