This requires us to perform a scan, just like the previous level. The difference here is that we need to scan on a much larger network. So we'll need to optimize our scan to enhance the performance. We can do the same thing as before, but this time we add in 2 new flags: `-T5` and `--min-parallelism`. `-T5` allows us to control the timing and speeds up the scan. `--min-parallelism` allows us to specify how many parallel scan to run at minimum, higher parallelism can cause inaccuracy in scan but in our case, we only need host discovery, not port scan, so it should be fine.

```bash
nmap -sP 10.0.0.0/16 -T5 --min-parallelism 3000
nc 10.0.255.115 31337
```

[Reference](https://nmap.org/book/man-performance.html)