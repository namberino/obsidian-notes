This level requires us to send an "is-at" ARP request manually. An ARP request will require the source IP address, destination IP address, source MAC address and destination MAC address. Since we're trying to figure out the MAC address, we set the destination MAC address to `ff:ff:ff:ff:ff:ff` to broadcast the ARP packet to all host, and we fill in the rest of the information.

```python
from scapy.all import *

eth = Ether(src="ea:58:03:8e:9e:60")
arp = ARP(psrc="10.0.0.2", pdst="10.0.0.3", hwsrc="ea:58:03:8e:9e:60", hwdst="ff:ff:ff:ff:ff:ff", op="is-at")
sendp(eth / arp, iface="eth0")
```