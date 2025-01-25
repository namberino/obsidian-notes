This level is pretty much the same as level 5, except the traffic is slower. The data in this level is not sent in 1 go, but in fragments. We can monitor it in `wireshark`.

```bash
wireshark
```

We do have to wait for the packets to come though. After waiting for a while and got enough packets, we got the data. However, the data looks like the flag but every characters has been typed twice. So to fix this, we can just whip up a quick Python script.

```python
package = "ppwwnn..ccoolllleeggee{{EEWW00JJXXyyooEEUU11ggKKoovvXXkkssxxbb11SSii0000ttmmWW..ddRRjjNNzzMMDDLL55AAjjNNzzkkzzWW}}"
unpacked = ""

for i in range(len(package)):
    if i % 2 == 0:
        unpacked += package[i]

print(unpacked)
```