This level requires us to send a TCP packet. This is pretty similar to our previous level, except we need to add on top of our IP packet another TCP packet. It's pretty simple to implement, we just need to make a `TCP` object with the correct parameters set.

```python
from scapy.all import *

eth = Ether(src="4e:e6:02:6b:bc:4a")
ip = IP(dst="10.0.0.3")
packet = eth / ip
tcp = TCP(sport=31337, dport=31337, seq=31337, ack=31337, flags="APRSF")
sendp(packet / tcp, iface="eth0")
```