#Table of Contents#

* Network Topology
* Description of Targets
* Monitoring the Targets
* Patterns of Traffic & Behavior
* Suggestions for Going Further
* 
#Network Topology#

The following machines were identified on the network:

|   Hostname    |        IPv4         |          OS          |
|---------------|---------------------|----------------------|
| ELK           | 192.168.1.100       |  Ubuntu              |
| Kali          | 192.168.1.90        |  Debian Kali         |
| Capstone      | 192.168.1.105       |  Ubuntu              |
| Target 1      | 192.168.1.110       |  Debian GNU/Linux 8  |

<img width="553" alt="186093586-2e8179b1-08aa-4c37-9513-7e2c2d8b3153" src="https://user-images.githubusercontent.com/100730516/186272917-354a3da5-223a-45d2-9158-bce6ee77c70d.png">

#Description of Targets#

The target of this attack was: Target 1 (192.168.1.110). Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

#Monitoring the Targets#

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

<b>Excessive HTTP Errors<b>

* <b>WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes.<b>

  * Metric: WHEN count() GROUPED OVER top 5 'http.response.status_code'

  * Threshold: IS ABOVE 400

  * Vulnerability Mitigated: Enumeration/ Brute Force

  * Reliability: This alert is highly reliable. Measuring the error code that are 400 and above filter out the normal activity or successful responses. 400 and plus codes are client and server errors which are ones of more concern. Especially, when the error codes happen at a high rate.

<img width="596" alt="Excessive-HTTP-Errors" src="https://user-images.githubusercontent.com/100730516/186273053-432ca205-edab-4b1c-9aad-97bd8922b52c.png">

<b>HTTP Request Size Monitor<b>

* WHEN sum() of http.request.byte OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute.

  * Metric: WHEN sum() of http.request.byte OVER all documents

  * Threshold: IS ABOVE 3500

  * Vulnerability Mitigated: Code injection in HTTP requests (XSS and CRLF) or DDOS

  * Reliability: This alert could create false positives, which set the alert at medium reliability. There is a possiblity for a large number of non-malicious HTTP requests or legitimate HTTP traffic. 

<img width="593" alt="HTTP-Request-Size-Monitor" src="https://user-images.githubusercontent.com/100730516/186273076-f85e8722-681b-4dcd-b8c2-e9631f0d1f02.png">

<b>CPU Usage Monitor<b>

* WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes

  * Metric: WHEN max() OF system.process.cpu.total.pct OVER all documents

  * Threshold: IS ABOVE 0.5

  * Vulnerability Mitigated: Malicious software, programs (malware or viruses) running taking up resources.

  * Reliability: This alert is highly reliable. Even if there isn't malicious programs running, this alert can still help determine where the CPU can improve usage.

<img width="599" alt="CPU-Usage-Monitor" src="https://user-images.githubusercontent.com/100730516/186273105-337ea9af-8c91-4408-8dfa-6c9761af09ee.png">

#Suggestions for Going Further (Optional)#
  
Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks. It is not necessary to explain how to implement each patch.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:

<b>Excessive HTTP Errors<b>

<b>Patch: WordPress Hardening<b>

* Regular updates should be implemented.

  * WordPress Core

  * PHP versions

  * Plugins

* Installing security plugin(s)

  * For example, Wordfence(added security funtionality)

* Disabling unused WordPress features and settings, like the following:

  * WordPress XML-RPC (on by default)

* Blocking request to /?author= by configuring web server settings.

* Remove the WordPress logins from being publicly accessible, specifically:

  * /wp-admin

  * /wp-login.php

<b>Why it Works<b>

* Regular updates to WordPress, the PHP version and plugin is a simple way to implement patches or fixes to the vulnerabilites/exploits.

* Depending on the WordPress Security Plugins, features like this are added:

  * Malware Scans

  * Firewall

* REST API is used by WPScan to enumerate users. Disabiling this will help mitigate WPScan or enumeration in general.

* XML-RPC uses HTTP as a method of data transportation.

* Removeral of public access to WordPress logins will help reduce the attacks.

<b>HTTP Request Size Monitor<b>

<b>Patch: Code Injection/ DDOS Hardening<b>

* Implenmentation of HTTP Requests Limit on the web server.

  * Limits can include:

    * Maximum URL length

    * Maximum size of requests

* Implementation of input validation on forms.

<b>Why It Works:<b>

* If an HTTP request length and over size limit of the request, a 404 error will occur.

  * This will help reject the requests that are too large.
* Input validation could help protect against attacks who attempt to send the server via the website or application accross a HTTP request.

<b>CPU Usage Monitor<b>

<b>Patch: Virus or Malware Hardening**<b>

  * Add or update current antivirus protection

  * Implenment and configure Host Based Intrusion Detection System (HIDS)

<b>Why It Works:<b>

* Antivirus protection specializes in removeral, detection and overall prevention of malicious threats against the computers.

* HIDS monitors and analyzes internals of the computing system.
