# THM_Web-Security-Monitoring

## Table of Contents
1. [Web Security Essentials](#web-security-essentials)
2. [Detecting Web Attacks](#detecting-web-attacks)

## Web Security Essentials
### Why Web?
1. Have applications shifted from desktop to web over the past couple of decades (Yea/Nay)?

    The answer is `Yea`.

2. Who is ultimately responsible for ensuring the security of users' data within a web application?

    The answer is `Web App Owner`.

### Web Infrastructure
1. What does your web browser send to a server to receive a web page?

    The answer is `Request`.

2. What web server is most commonly used to host WordPress websites?

    The answer is `Apache`.

3. What do we call the OS and environment that runs the web server and application?

    The answer is `Host Machine`.

### Protecting the Web
1. What cyber security concept involves stopping or limiting damage from threats?

    The answer is `Mitigation`.

2. What security control involves ensuring all software and components are up to date?

    The answer is `Patch Management`.

### Defense System
1. Which type of Web Application Firewall operates by running on the same system as the application itself?

    The answer is `Host-based`.

2. Which common WAF detection technique works by matching incoming requests against known malicious patterns?

    The answer is `Signature-based`.

### Practice Scenario
1. What flag did you receive for securing the Web Application?

    The answer is `THM{web_app_secured!}`.

2. What flag did you receive for securing the Web Server?

    The answer is `THM{server_security_expert!}`.

3. What flag did you receive for securing the Host Machine?

    The answer is `THM{the_final_security_layer!}`.


## Detecting Web Attacks
### Client-Side Attacks
1. What class of attacks relies on exploiting the user's behavior or device?

    The answer is `Client-Side`.

2. What is the most common client-side attack?

    The answer is `XSS`.

### Server-Side Attacks
1. What class of attacks relies on exploiting vulnerabilities within web servers?

    The answer is `Server-Side`.

2. Which server-side attack lets attackers abuse forms to dump database contents?

    The answer is `SQLi`.

### Log-Based Detection
1. What is the attacker's User-Agent while performing the directory fuzz?

    The answer is `FFUF v2.1.0`.

2. What is the name of the page on which the attacker performs a brute-force attack?

    The answer is `/login.php`.

3. What is the complete, decoded (opens in new tab) SQLi payload the attacker uses on the /changeusername.php form?

    The answer is `%' OR '1'='1`.

### Network-Based Detection
1. What password does the attacker successfully identify in the brute-force attack?

    To solve this, we can use this filter in wireshark to find successful login attempts with response 302:

    ```
    http.response.code == 302
    ```
    Then, we can right click on the packet and select "Follow" > "HTTP Stream" to see the complete request and response. The password will be visible in the request payload. The answer is `astrongpassword123`.

2. What is the flag the attacker found in the database using SQLi?

    To find the flag, we can add `user-agent` column and search for `sqlmap` agent. Then, we can right click on the packet and select "Follow" > "HTTP Stream" to see the complete request and response. The flag will be visible in the response payload. The answer is `THM{dumped_the_db}`.

### Web Application Firewall
1. What do WAFs inspect and filter?

    The answer is `Web Requests`.

2. Create a custom firewall rule to block any User-Agent that matches "BotTHM".

    The answer is `IF User-Agent CONTAINS "BotTHM" THEN block`.
    