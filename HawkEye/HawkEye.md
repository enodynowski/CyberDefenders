# Solutions to HawkEye from Cyberdefenders.org
## Contents
1. [Question 1](#question-1)
2. [Question 2](#question-2)
3. [Question 3](#question-3)
4. [Question 4](#question-4)
5. [Question 5](#question-5)
6. [Question 6](#question-6)
7. [Question 7](#question-7)
8. [Question 8](#question-8)
9. [Question 9](#question-9)
10. [Question 10](#question-10)
11. [Question 11](#question-11)
12. [Question 12](#question-12)
13. [Question 13](#question-13)
14. [Question 14](#question-14)
15. [Question 15](#question-15)
16. [Question 16](#question-16)
17. [Question 17](#question-17)
18. [Question 18](#question-18)
19. [Question 19](#question-19)
20. [Question 20](#question-20)
21. [Question 21](#question-21)
22. [Question 22](#question-22)
23. [Question 23](#question-23)
24. [Question 24](#question-24)
25. [Question 25](#question-25)
26. [Question 26](#question-26)


## <b> 1. How many packets does the capture have?</b> <a name = "question-1"> </a>
Open the file in Wireshark and scroll to the bottom.
<details>
    <summary> <b>Solution </b> </summary>
    4003
</details>


## <b> 2. At what time was the first packet captured?</b> <a name = "question-2"> </a>
Click the first packet and under the ``Frame 1`` tab select ``Arrival Time``
<details>
    <summary> <b>Solution </b> </summary>
    2019-04-10 20:37:07 UTC
</details>


## <b> 3. What is the duration of the capture?</b> <a name = "question-3"> </a>
Subtract the value of [Question 2](#question-2) from the same value in the last packet. 
<details>
    <summary> <b>Solution </b> </summary>
    01:03:41
</details>


## <b> 4. What is the most active computer at the link level?</b> <a name = "question-4"> </a>
In Wireshark go to: ``Statistics`` -> ``Conversations``

Then select Ethernet, and sort by packets.
<details>
    <summary> <b>Solution </b> </summary>
    00:08:02:1c:47:ae
</details>


## <b> 5. What is the manufacturer of the NIC of the most active machine?</b> <a name = "question-5"> </a>
A quick google search of the solution to [Question 4](#question-4) 
<details>
    <summary> <b>Solution </b> </summary>
    Hewlett-Packard
</details>



## <b> 6. Where is the headquarter of the company that manufactured the NIC of the most active computer at the link level?</b> <a name = "question-6"> </a>
A quick google search of the solution to [Question 5](#question-5) 
<details>
    <summary> <b>Solution </b> </summary>
    Palo Alto
</details>


## <b> 7. The organization works with private addressing and netmask /24. How many computers in the organization are involved in the capture?</b> <a name = "question-7"> </a>
In Wireshark again, go to: ``Statistics`` -> ``Endpoints``

Then look through the IPv4, UDP, TCP, and Ethernet tabs and count the hosts that are within the private IP range. 
<details>
    <summary> <b>Solution </b> </summary>
    3
</details>


## <b>8. What is the name of the most active computer at the network level?</b> <a name = "question-8"> </a>
Filter for DHCP traffic and choose one of the two resulting packets. In the DHCP data is the host name
<details>
    <summary> <b>Solution </b> </summary>
    Beijing-5cd1-PC
</details>



## <b>9.What is the IP of the organization's DNS server?</b> <a name = "question-9"> </a>
Filter for DNS traffic. 
<details>
    <summary> <b>Solution </b> </summary>
    10.4.10.4
</details>


## <b>10. What domain is the victim asking about in packet 204?</b> <a name = "question-10"> </a>
Navigate to packet 204, and in the DNS header go to ``Queries``
<details>
    <summary> <b>Solution </b> </summary>
    proforma-invoices.com
</details>



## <b>11. What is the IP of the domain in the previous question?</b> <a name = "question-11"> </a>
In packet 206 is the DNS response to the request in packet 204. Again, the ``Queries`` tab contains the address. 
<details>
    <summary> <b>Solution </b> </summary>
    217.182.138.150
</details>

## <b>12. Indicate the country to which the IP in the previous section belongs.</b> <a name = "question-12"> </a>
Run the command

    whois
on the IP address found in [Question 11](#question-11)
<details>
    <summary> <b>Solution </b> </summary>
    France
</details>


## <b>13. What operating system does the victim's computer run?</b> <a name = "question-13"> </a>
Filtering for HTTP traffic. In the GET Header of packet 210, the User Agent string specifies the operating system 
<details>
    <summary> <b>Solution </b> </summary>
    Windows NT 6.1
</details>


## <b>14. What is the name of the malicious file downloaded by the accountant?</b> <a name = "question-14"> </a>
In the same packet as above, the malicious file is downloaded via HTTP, it is supplied in the request headers. 
<details>
    <summary> <b>Solution </b> </summary>
     tkraw_Protected99.exe
</details>


## <b>15. What is the md5 hash of the downloaded file?</b> <a name = "question-15"> </a>
Wireshark was uncooperative, so I used tshark. I ran the command:

    tshark -r stealer.pcap --export-objects http,.
This gave me the malicious file. I ran:
    
    md5sum {FILENAME}.exe
<details>
    <summary> <b>Solution </b> </summary>
    71826ba081e303866ce2a2534491a2f7
</details>


## <b>16. What is the name of the malware according to Malwarebytes?</b> <a name = "question-16"> </a>
I ran the file hash from the above question in ``virustotal.com``, then dug around for the malwarebytes alert. 

<details>
    <summary> <b>Solution </b> </summary>
    Spyware.HawkEyeKeyLogger
</details>


## <b>17. What software runs the webserver that hosts the malware?</b> <a name = "question-17"> </a>
Looking in the HTTP traffic again. Packet 3155...
<details>
    <summary> <b>Solution </b> </summary>
    LiteSpeed
</details>


## <b>18. What is the public IP of the victim's computer? </b> <a name = "question-18"> </a>
digging through the HTTP traffic, theres some to a website called ``bot.whatismyipaddress.com``. Following the HTTP stream gives the solution. 
<details>
    <summary> <b>Solution </b> </summary>
    173.66.146.112
</details>


## <b>19. In which country is the email server to which the stolen information is sent?</b> <a name = "question-19"> </a>
On a hunch, I filtered for SMTP. Of the 2 addresses that appear, only one is public. I ran this command:

    whois 23.229.162.69
<details>
    <summary> <b>Solution </b> </summary>
    United States
</details>


## <b>20. What is the domain's creation date to which the information is exfiltrated?</b> <a name = "question-20"> </a>
Again, looking at the SMTP traffic. Lots of the emails are being sent to a @macwinlogistics.in email. Running

    whois macwinlogistics.in

Gives us the creation date. 
<details>
    <summary> <b>Solution </b> </summary>
    2014-02-08
</details>


## <b>21. Analyzing the first extraction of information. What software runs the email server to which the stolen data is sent?</b> <a name = "question-21"> </a>
Following the TCP stream of any of the SMTP traffic and it's in the very first line. 
<details>
    <summary> <b>Solution </b> </summary>
    Exim 4.91
</details>


## <b>22. To which email account is the stolen information sent?</b> <a name = "question-22"> </a>
Again, just following the TCP stream of the SMTP traffic yields this information.
<details> 
    <summary> <b>Solution </b> </summary>
    sales.del@macwinlogistics.in
</details>


## <b>23. What is the password used by the malware to send the email?</b> <a name = "question-23"> </a>
Still in the SMTP traffic - Packet 3933 indicates that its a password. Looks like base64. [Cyberchef](https://gchq.github.io/CyberChef/) says that the password is:
<details>
    <summary> <b>Solution </b> </summary>
    Sales@23
</details>


## <b>24. Which malware variant exfiltrated the data?</b> <a name = "question-24"> </a>
The subject line of the email sent via SMTP contains the answer, encoded in base64. 
<details>
    <summary> <b>Solution </b> </summary>
    Reborn v9
</details>


## <b>25. What are the bankofamerica access credentials? (username:password)</b> <a name = "question-25"> </a>
Looking at the large chunk of base64 encoded data, and using [Cyberchef](https://gchq.github.io/CyberChef/) again gives a nice litte table containing all the information
<details>
    <summary> <b>Solution </b> </summary>
    roman.mcguire:P@ssw0rd$
</details>


## <b>26. Every how many minutes does the collected data get exfiltrated?</b> <a name = "question-26"> </a>
Looking at all of the SMTP traffic all together, an email is sent in every SMTP/IMF packet. Just look at the time difference between each. 
<details>
    <summary> <b>Solution </b> </summary>
    10
</details>
