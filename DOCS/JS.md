# JavaScript (JS) — Essentials & Security Notes

**Summary:** JavaScript is an interpreted language executed in the browser. It’s used for dynamic interaction and UI logic. When pen-testing, check whether JS is internal or external since that affects attack surfaces (e.g. XSS, malicious external libs). :contentReference[oaicite:14]{index=14}

## Internal vs External JS
- **Internal JS:** embedded inside `<script>` tags in HTML. Good for small scripts or demo code.
- **External JS:** stored in `.js` files and referenced with `<script src="...">`. Keeps HTML clean and scalable.

### Example (Internal)
```html
<script>
  let x = 5;
  let y = 10;
  document.getElementById("result").innerHTML = "The result is: " + (x + y);
</script>
