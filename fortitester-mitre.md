
## FortiTester Question to Baojun
Questions :
1. If we want to demo FTS MITRE ATTACK capability, should we use v6 or v10+ Beta
2. In v6 : 311 and in v10+ : 340, so more on v10+ ?
3. In v10, what is the usage of Exfil Server, is there any ExFil? I can only see there is one ExFil outside using file.io, are there any other "Abilities" that can do exfil to other network segment ?
4. If we are using this demo, is it best to demo FortiEDR and FortiGate ? Or it is more suited for FortiEDR so it detects 'anomalies' in the host installed ?
5. In Attack v6, there are RATs, but in Attack v10+ Beta, there is no more RATs. Also in previous v6 we can specify Active User/Logon User, also enable Windows Defender and Firewall..

But in v10 it is very minimal, So what's the best ?

![[Pasted image 20221031141125.png]]

![[Pasted image 20221031141242.png]]

# Testing On Linux
![[Pasted image 20221031214832.png]]

Have issues with activate.py, so manually run the fortiagent
Seems to be working.

What is the best way to demo this MITRE ATT&CK ?
I can see like "Bitsadmin Download" -> what is the objective for this action? Should bitsadmin be blocked ? ( after googling found out that bitsadmin is background download and process only see as svchosts.exe )

# FortiAgent in windows

It looks like we can not use different ports than 443. In Linux this is possible !

![[Pasted image 20221101212147.png]]

# Trick Use FGT VIP

```
FGT-TASHOME (vip) # show
config firewall vip
    edit "FTS-MITRE-TEST"
        set uuid 8a5cff90-59fe-51ed-306d-a85cb57be513
        set extip 12.34.56.78
        set mappedip "34.128.85.232"
        set extintf "internal"
        set portforward enable
        set color 23
        set extport 443
        set mappedport 14000
    next
end

config firewall policy
.
.

    edit 8
        set name "Trick-VIP-FTS"
        set uuid bc3913be-59fe-51ed-b02d-64c4eccd31ad
        set srcintf "internal"
        set dstintf "virtual-wan-link"
        set action accept
        set srcaddr "all"
        set dstaddr "FTS-MITRE-TEST"
        set schedule "always"
        set service "ALL"
        set logtraffic all
        set nat enable
    next
end

C:\Users\Public>type conf.yml
ip: 12.34.56.78
paw: dogvoeigrg

```
>[!info]
>This no longer required with fortiagent 2.0.1 that is given by Owen



## Mimikatz trigger Windows Defender
![[Pasted image 20221101233243.png]]

# Capture WebCam also trigger Windows Defender
![[Pasted image 20221101234753.png]]

## Capture WebCam works
(after disabling real-time windows defender)

```
C:\Users\Public>dir
 Volume in drive C is New Volume
 Volume Serial Number is CC98-2FCD

 Directory of C:\Users\Public

11/02/2022  01:32 PM    <DIR>          .
11/02/2022  01:32 PM    <DIR>          ..
04/18/2021  05:27 PM    <DIR>          BlueStacks
11/02/2022  01:15 PM                34 conf.yml
01/19/2022  04:03 PM    <DIR>          Documents
12/07/2019  04:14 PM    <DIR>          Downloads
11/01/2022  10:30 AM         7,299,048 fortiagent-windows.exe
11/02/2022  01:33 PM             9,059 fortiagent.log
05/17/2022  02:23 AM            11,358 LICENSE-2.0.txt
12/07/2019  04:14 PM    <DIR>          Music
12/07/2019  04:14 PM    <DIR>          Pictures
11/02/2022  01:32 PM           921,654 qmi3wc56_T1125.bmp
12/07/2019  04:14 PM    <DIR>          Videos

```

![[Pasted image 20221102133510.png]]

## New Fortiagent-windows.exe 2.0.1 given by Owen 3/11/2022

![[Pasted image 20221103192751.png]]
>[!info]
It works... so this 2.0.1 can help if the FortiTester is not using standard port https 


