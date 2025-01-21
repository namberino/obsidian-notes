This level requires us to break out of the `<textarea>` tag and execute the `alert("PWNED")` command. Since the `<textarea>` is enclosed with a closing tag `</textarea>`, we can just close it kinda like how we would do with an SQL injection and insert our script:

```
http://challenge.localhost/?msg=%3C%2Ftextarea%3E%3Cscript%3Ealert%28%22PWNED%22%29%3C%2Fscript%3E
```