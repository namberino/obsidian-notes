This requires us to make an XSS script that would force the victim to make a request to publish their drafts. The drafts will only show the first 12 characters of the post. The user can publish all of their drafts using the `Publish drafts` button.

When the `Publish drafts` button is pressed, the website makes a GET request to `http://challenge.localhost/publish`. So what we need to do is to craft an XSS script that would send a GET request to this endpoint. This would force the victim to publish all their drafts and give us the flag.

```html
<script>fetch("http://challenge.localhost/publish", {method:"GET"})</script>
```