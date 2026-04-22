# THM_Web-Security-Monitoring

## Table of Contents
1. [Web Security Essentials](#web-security-essentials)
2. [Detecting Web Attacks](#detecting-web-attacks)
3. [Detecting Web Shells](#detecting-web-shells)
4. [Detecting Web DDoS](#detecting-web-ddos)

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
    

## Detecting Web Shells
### Web Shell Overview
1. Which MITRE ATT&CK Persistence sub-technique are web shells associated with?

    We can check this [website](https://attack.mitre.org/techniques/T1505/003/). The answer is `T1505.003`.

2. What file extension is commonly used for web shells targeting Microsoft Exchange?

    The answer is `.aspx`.

### Anatomy of a Web Shell
1. Access the shell and determine which account you have access to by running the whoami command.

    The answer is `www-data`.

2. List the directory contents and find the flag using the ls and cat commands.

    The answer is `THM{W3b_Sh3ll_Usag3}`.

### Log-Based Detection
1. What is the part of the URL that associates values to parameters and can be a valuable indicator of web shell activity?

    The answer is `Query String`.

2. What auditd syscall would confirm that a file was written to disk following a suspicious POST request to /upload.php?

    The answer is `creat`. 

### Beyond Logs
1. What command would you use to locate .php files in the /var/www/ directory?

    The answer is `find /var/www/ -type f -name "*.php"`.

2. Which Wireshark filter would you use to search specifically for PUT requests?

    The answer is `http.request.method == "PUT"`.

### Investigation
1. Which IP address likely belongs to the attacker?

    We can use this command to filter `php` result:

    ```bash
    cat /var/log/apache2/access.log | grep php
    ```
    Then, we can look for the IP address that is making suspicious requests like repeated GET requests with `404` response code and suspicious User-Agent. The answer is `203.0.113.66`.

2. What is the first directory that the attacker successfully identifies?

    We can filter to the attacker's IP address and look for the first successful request with `200` response code:

    ```bash
    cat /var/log/apache2/access.log | grep 203.0.113.66 | grep 200
    ```
    The answer is `/wordpress`.

3. What is the name of the .php file the attacker uses to upload the web shell?

    We can filter to the attacker's IP address, POST method, word "wordpress", and ".php" in the URL:

    ```bash
    cat /var/log/apache2/access.log | grep 203.0.113.66 | grep wordpress | grep php | grep POST
    ```
    The answer is `upload_form.php`.

4. What is the first command run by the attacker using the newly uploaded web shell?

    We can filter to the attacker's IP address, `.php` file extension, and `cmd` parameter in the query string:

    ```bash
    cat /var/log/apache2/access.log | grep 203.0.113.66 | grep wordpress | grep php | grep cmd 
    ```
    Then, we can look for the first command in the query string. The answer is `whoami`.

5. After gaining access via the web shell, the attacker uses a command to download a second file onto the server. What is the name of this file?

    We can use the previous filter and look for the second command in the query string that includes a file download command like `wget` or `curl`. In this case, the attacker used `wget` to download the file. The answer is `linpeas.sh`.

6. The attacker has hidden a secret within the web shell. Use cat to investigate the web shell code and find the flag.

    First, we can use `find` command to locate the web shell file:

    ```bash
    find / -name "shadyshell.php" 2>/dev/null
    ```
    Then, we can use `cat` command to read the contents of the web shell file:

    ```bash
    cat /var/www/html/wordpress/wp-content/uploads/shadyshell.php
    ```
    The answer is `THM{W3b_Sh3ll_Int3rnals}`.


## Detecting Web DDoS
### DoS and DDoS Attacks
1. What class of attack relies on disrupting the availability of a web service?

    The answer is `Denial-of-Service`.

2. What do we call the network of compromised machines that attackers use to launch DDoS attacks?

    The answer is `Botnet`.

### Attack Motives
1. Which attacker motive aims to make customers lose confidence in a company?

    The answer is `Reputational Damage`.

2. Which motive most likely drove the 2023 DDoS attack against Microsoft?

    The answer is `Hacktivism`.

### Log Analysis
1. What is the attacker’s IP address?

    We can examine the log file for a high volume of requests from a single IP address. The answer is `203.12.23.195`.

2. Which page is repeatedly targeted by the attacker’s requests?

    We can look for the most frequently requested page in the log file from ip `203.12.23.195`. The answer is `/login`.

3. After the attack, what error code do legitimate users receive?

    We can look for the response codes in the log file after the attack. The answer is `503`.

### Leveraging SIEMs
1. What was the most frequently requested uri?

    We can use the SIEM's query capabilities to analyze the log data and identify the most frequently requested URI by using this query:

    ```
    index="main" | timechart span=1m count by uri
    ```
    Then, we can go to visualization and look for the most frequently requested URI. The answer is `/search`.

2. Which clientip made the most requests to the target uri?

    We can use this query to find the client IP that made the most requests to the target URI:

    ```
    index="main" uri="/search" | top clientip
    ```
    The answer is `203.0.113.7`.

3. How many IP addresses were part of the botnet that attacked your website?

    First, we can filter based on time range of the attack. Then we can use this query to find the number of unique client IP addresses that made requests to the target URI:

    ```
    index="main" uri="/search" | stats dc(clientip) as unique_ips
    ```
    The answer is `60`.

4. Which useragent was most commonly used by the attacking traffic?

    We can use this query to find the most common user agent used by the attacking traffic:

    ```
    index="main" uri="/search" | top useragent
    ```
    The answer is `Java/1.8.0_181`.

5. Use the timechart command to visualize the requests. What is the peak number of requests made per second during the attack?

    We can use this query to visualize the requests and find the peak number of requests per second:

    ```
    index="main" uri="/search" | timechart span=1s count
    ```
    Then, we can go to visualization and look for the peak value in the timechart. The answer is `207`.

6. Which legitimate (non-attacking) clientip received the first 503 response status post-attack?

    Based on the previous analysis, the attack traffic came from `13:11:00` to `13:12:1`. So, we can filter the time range to after the attack and look for the first 503 response status from a non-attacking client IP:

    ```
    index="main" status=503 | sort -_time desc
    ```
    The answer is `10.10.0.27`.

### Defense
1. What type of security challenge blocks bots by asking users to solve a simple puzzle?

    The answer is `CAPTCHA`.

2. Which CDN feature spreads traffic across multiple servers to prevent overload?

    The answer is `Load-balancing`.