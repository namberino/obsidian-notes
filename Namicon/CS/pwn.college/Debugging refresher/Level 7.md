This level is straight forwards, GDB has full control over the process it's attached to. We can actually call the functions in the attached process in GDB.

```
call (void)win()
```

Because GDB cannot attached to a privileged process for a normal user, it's not a security risk. 