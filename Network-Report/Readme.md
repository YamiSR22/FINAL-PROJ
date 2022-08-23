# Network Analysis #

### Time Thieves ###

At least two users on the network have been wasting time on YouTube. Usually, IT wouldn't pay much mind to the behavior, but it seems these people have created their own web server on the corporate network. So far, Security knows the following about these time thieves:

* They have set up an Active Directory network
* They are constantly watching videos on YouTube.
* Their IP addresses are somewhere in the range 10.6.12.0/24

The traffic must be inspected to answer the following Network Report

1. What is the domain name of the users' custom site?

  * The Domain Name: Frank-n-Ted-DC.frand-n-ted.com.

  * Filter used in Wireshark: ip.addr==10.6.12.0/24

  * Results:
<img width="577" alt="Frank-n-Ted-DC" src="https://user-images.githubusercontent.com/100730516/186276737-e5061f19-c92d-4af7-9df6-d975f2a2f552.png">

2. What is the IP address of the Domain Controller (DC) of the AD network?

  * IP address is 10.6.12.12 (Frank-n-Ted-DC.frank-n-ted.com)

  * Filter used in Wireshark: ip.addr==10.6.12.0/24

  * Results:
<img width="577" alt="Domain-Controller-of-AD" src="https://user-images.githubusercontent.com/100730516/186276816-7fcabe7b-b28e-4cc9-b440-891579218a77.png">

3. What is the name of the malware downloaded to the 10.6.12.203 machine?

  * Malware file: june11.dll

  * Results:
<img width="575" alt="june11" src="https://user-images.githubusercontent.com/100730516/186276860-7ffab9a8-eac1-48dc-9e97-3fa0e7234748.png">

* Once the file is found, the file was exported to the Kali machine.

* Filter used in Wireshark: ip.addr==10.6.12.203 and http.request.method==GET

4. Upload the file to VirusTotal.com

  * This type of malware is classified as a Trojan

  * Results:
<img width="582" alt="virustotal-file-upload" src="https://user-images.githubusercontent.com/100730516/186276904-bc0f567e-482f-4a3a-aaf5-bde0d2e783e5.png">

<img width="570" alt="Trojan-VirusTotal" src="https://user-images.githubusercontent.com/100730516/186276922-01a5411a-2958-42d1-825a-b756da4a5c62.png">

# Vulnerable Windows Machines #

The Security Team received reports of an infected Windows host on the network. They know the following:

* Machines in the network live in the range 172.164.0/24.

* The domain mind-hammer.net is associated with the infected computer.

* The DC for this network lives at 172.16.4.4 and is named Mind-Hammer-DC.

* The network has standard gateway and broadcast addresses.

Inspect the traffic to answer the following questions:

1. Find the following information about the infected Windows machine:

  * Host name: ROTTERDAM-PC

  * IP address: 172.16.4.205

  * MAC addrss: 00:59:07:b0:63:a4

  * Filter used in Wireshark: ip.src==172.16.4.4 and kerberos.CNameString

<img width="572" alt="ROTTERDAM-PC" src="https://user-images.githubusercontent.com/100730516/186277029-43ba8a16-12ea-4ff6-93b0-3034add9de2a.png">

2. What is the username of the Windows user whose computer is infected?

  * Filter used in Wireshark: ip.src==172.16.4.205 and kerberos.CNameString

3. What are the IP addresses used in the actual infection traffic?

  * Based on the Conversation statistics and the filtering by the highest amount of packets between the IP addresses- 172.16.4.205, 185.243.115.84, 166.62.11.64 are the infected traffic.

<img width="590" alt="ACTUAL-INFECTION-TRAFFIC" src="https://user-images.githubusercontent.com/100730516/186277115-14ebe6d0-7115-46a2-8930-8239bb7f15ef.png">

4. As a bonus, retrieve the desktop background of the Windows host.

<img width="578" alt="search-picture" src="https://user-images.githubusercontent.com/100730516/186277166-cfffde08-ecdf-48ad-9949-8ddcf698d2b8.png">

![actual-picture](https://user-images.githubusercontent.com/100730516/186277191-0df47405-5dc9-4521-b566-2dae64077656.png)

