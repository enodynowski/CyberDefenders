# Solutions to BSidesJeddah-Part2 from Cyberdefenders.org
## Contents
1. [Question 1](#question-1)
2. [Question 2](#question-2)
10. [Question 10](#question-10)

## <b> 1. What is the SHA256 hash value of the RAM image?</b> <a name = "question-1"> </a>
    sha256sum memory.mem
<details>
    <summary> <b>Solution </b> </summary>
    5b3b1e1c92ddb1c128eca0fa8c917c16c275ad4c95b19915a288a745f9960f39
</details>

## <b> 2. What time was the RAM image acquired according to the suspect system?</b> <a name = "question-2"> </a>
For this question, I used volatility 3, rather than volatility 2. I used the below command to get the time:

    vol -f memory.mem windows.info
<details>
    <summary> <b>Solution </b> </summary>
    2021-08-06 16:13:23
</details>

## <b> 3. What volatility2 profile is the most appropriate for this machine? </b>
Back to volatility 2 for this question. I used the below command to get the profile. It takes a while so be patient:

    vol.py -f memory.mem imageinfo
<details> 
    <summary> <b>Solution </b> </summary>
    Win2016x64_14393
</details>

## <b> 4. What is the computer's name? </b>
There were multiple ways to do this. I examined the registry and used the following commands to get the computer's name:

    vol.py -f memory.mem --profile=Win2016x64_14393 hivelist

I then used hivedump with the offset found from the previous command, and used grep to find the computer's name:

    vol.py -f memory.mem --profile=Win2016x64_14393 hivedump -o 0xffff808fe7e41000 | grep "ComputerName"

After finding the ComputerName hive key in the registry, I used the following command to get the computer's name:

    vol.py -f memory.mem --profile=Win2016x64_14393 printkey -o 0xffff808fe7e41000 -K “ControlSet001\Control\ComputerName\ComputerName”
<details>
    <summary> <b>Solution </b> </summary>
    WIN-8QOTRH7EMHC
</details>

## <b> 5. What is the system IP address? </b>
Continuing to use volatility 2 here. I used the following command to get the system IP address:

    vol.py -f memory.mem --profile=Win2016x64_14393 netscan
<details>
    <summary> <b>Solution </b> </summary>
    192.168.144.131
</details>

## <b> 6. How many established network connections were at the time of acquisition? </b>
I used the following command to get the number of established network connections:

    vol.py -f memory.mem --profile=Win2016x64_14393 netscan | grep "ESTABLISHED" | wc -l 
<details>  
    <summary> <b>Solution </b> </summary>
    12
</details>

## <b> 7. What is the PID of explorer.exe? </b>
I used the following command to get the PID of explorer.exe:

    vol.py -f memory.mem --profile=Win2016x64_14393 pslist | grep "explorer.exe" | awk '{print $2}'
<details>
    <summary> <b>Solution </b> </summary>
    2676
</details>

## <b> 8. What is the title of the webpage the admin visited using IE? </b>
I used the following command to get the title of the webpage the admin visited using IE:

    vol.py -f memory.mem --profile=Win2016x64_14393 iehistory
<details>
    <summary> <b>Solution </b> </summary>
    Google News
</details>

## <b>9. What company developed the program used for memory acquisition? </b>
Back to volatility 3 for this question. I used the windows.cmdline plugin to determine the programs ran from the command line at the time of acquisition:

    vol -f memory.mem windows.cmdline

This reveals a process called "ramcapture64.exe" was run. Some cursory googling reveals that this program was developed by: 
<details>
    <summary> <b>Solution </b> </summary>
    Belkasoft
</details>

## <b> 10. What is the administrator user password? </b> <a name = "question-10"> </a>
Continuing with volatility 3, I used the following command to get the administrator user password:

    vol -f memory.mem windows.hashdump.Hashdump

this gives us the NTLM hash of the administrator's password. Running them through crackstation.net reveals the password is:
<details>
    <summary> <b>Solution </b> </summary>
    52(dumbledore)oxim
</details>

