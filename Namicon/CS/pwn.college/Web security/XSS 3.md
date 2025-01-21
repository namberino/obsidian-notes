This level wants us to craft an XSS URL and give it to the victim. The website will take in any input, put it into a URL parameter called `msg` and display the URL parameter `msg` onto the DOM.

We can create this XSS URL to trick the user into executing our XSS script that will execute `alert("PWNED")`.

```
http://challenge.localhost:80/?msg=<script>alert('PWNED')</script>
```