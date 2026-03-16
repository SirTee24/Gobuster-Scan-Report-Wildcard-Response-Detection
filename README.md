# Gobuster-Scan-Report-Wildcard-Response-Detection
Gobuster directory scan against http://tryhackme.com fails with a wildcard response error. 



A directory enumeration scan was performed against http://tryhackme.com using Gobuster. The scan was interrupted due to the server returning redirect responses for non-existent URLs, indicating a wildcard configuration.

🔍 Scan Details
Parameter	Value
Target URL	http://tryhackme.com
Tool	Gobuster v3.8.2
Mode	Directory Enumeration
Wordlist	/usr/share/wordlists/rockyou.txt.gz
Method	GET
Threads	10
Status Code Filter	Excluding 404
🚨 Error Encountered
text
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://tryhackme.com
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/rockyou.txt.gz
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
Progress: 0 / 1 (0.00%)
2026/03/16 21:29:43 the server returns a status code that matches the provided options for non existing urls. http://tryhackme.com/4e79e68c-a972-42f1-a09e-c56f5a3a05a1 => 301 (redirect to https://tryhackme.com/4e79e68c-a972-42f1-a09e-c56f5a3a05a1) (Length: 0). Please exclude the response length or the status code or set the wildcard option.. To continue please exclude the status code or the length
📊 Analysis
What Happened?
Gobuster sent a request to a randomly generated path:

text
http://tryhackme.com/4e79e68c-a972-42f1-a09e-c56f5a3a05a1
The server responded with:

Status Code: 301 (Redirect)

Location: https://tryhackme.com/4e79e68c-a972-42f1-a09e-c56f5a3a05a1

Response Length: 0

Why This Is a Problem
Since the server returns a 301 redirect (not a 404) for non-existent paths, Gobuster detects this as a wildcard response. This means:

Every directory in the wordlist would appear to exist

The scan results would be 100% false positives

Gobuster halts by default to prevent misleading results

✅ Recommended Solutions

Option 1: Scan HTTPS Directly (Recommended)
bash
gobuster dir -u https://tryhackme.com -w /usr/share/wordlists/rockyou.txt.gz
Option 2: Blacklist the 301 Status Code
bash
gobuster dir -u http://tryhackme.com -w /usr/share/wordlists/rockyou.txt.gz -b 301
Option 3: Exclude by Response Length
bash
gobuster dir -u http://tryhackme.com -w /usr/share/wordlists/rockyou.txt.gz --exclude-length 0
Option 4: Force Wildcard Mode (Use with Caution)
bash
gobuster dir -u http://tryhackme.com -w /usr/share/wordlists/rockyou.txt.gz --wildcard
📝 Additional Notes
The target forces HTTPS redirection, making Option 1 the most efficient approach

Using -b 301 will ignore all redirects, potentially missing legitimate redirects

The --wildcard flag will proceed with the scan but results may contain many false positives
