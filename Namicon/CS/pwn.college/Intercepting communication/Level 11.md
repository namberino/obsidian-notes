This requires us to manually perform the TCP 3-way handshake. We'll need to do the 3 parts of a TCP handshake manually. `scapy` allows us to do this through `srp1`. We can define the SYN flag with `flags='S'`, then use `srp1` to send the SYN packet and receive the SYNACK packet, once we've received the SYNACK packet, we can send an ACK packet with the acknowledgement parameter increased by 1 and the `flags='A'`.

```python
from scapy.all import *

eth = Ether(src="4e:e6:02:6b:bc:4a")
ip = IP(dst="10.0.0.3")
packet = eth / ip
syn = TCP(sport=31337, dport=31337, seq=31337, flags='S')
synack = srp1(packet / syn, iface="eth0")
ack = TCP(sport=31337, dport=31337, seq=synack.ack, ack=synack.seq+1, flags='A')
sendp(packet / ack, iface="eth0")
```