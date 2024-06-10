## Challenge:  The Email Where It Happens [Forensics] 150 Points

Howdy Truth Seekers! It seems that some malware that was strategically shared has begun to phone back home! We believe that this might have some very important information that could help lead us to finally getting to the bottom of this conspiracy regarding extraterrestrial life. Unfortunately the original developer of this tool was recently promoted to customer status and is no longer on good terms with the orginization. This means that we don't have any information on how to decode this traffic. Unfortunately all I have is a PCAP. Can you help us out here?

## Solution
### Step 1: Open the intercepted_communications.pcap
```
sudo wireshark intercepted_communications
```
Notice that the domain names appear to contain an encoded message

### Step 2: Dump the domain names into a text file
```
tshark -r intercepted_communication.pcap -Y "dns.qry.name" -T fields -e dns.qry.name > flagdata.txt
```
### Step 3: Decode
First, I attempted a base64 decode but then I realized that the encoded message was in all caps.  Base32 uses all caps.  For more information, see: https://stackoverflow.com/questions/54263836/how-to-do-base32-encoding-in-python3
    
I wrote the following script to decode:
```
import base64

filename = 'flagdata.txt'
with open(filename, "r") as f:
    data = f.read()
lines = data.split("\n")

encoded_msg = ''
for line in lines:
    domainames = line.split(".")
    encoded_msg += domainames[0]

print(encoded_msg)
msg = base64.b32decode(encoded_msg)
print(msg)
```
Result:
```
base32 encoded:
FIQSUIJKEEVCCKRBFIQSUIJKEEVAUSKOKRCVEQ2FKBKEKRBAIVGEKQ2UKJHU4SKDEBGUCSKMEBBU6TKNKVHESQ2BKREU6TQKFIQSUIJKEEVCCKRBFIQSUIJKEEVAUVCPHIQGE4TJMFXC44TJM5TXGQDBOJSWCNJRFZRWY33VMQFEMUSPJU5CA2LMNR2W22LOMF2GSQDTMVRXEZLUOMXHK4YKIRAVIRJ2EA2S6MJUF4ZDAMRUIAYTMORTGMFFGVKCJI5CAUSFHIQEGT2OIZEVETKFIQQEKWCUKJAVIRKSKJCVGVCSJFAUYICMJFDEKCSCJ5CFSOQKJBSWY3DPEBBHE2LBNYWAUCSXMUQGC4DQOJSWG2LBORSSA5DIMUQGS3TGN5ZG2YLUNFXW4IDZN52SA2DBOZSSA4DSN53GSZDFMQQHK4ZAOJSWOYLSMRUW4ZZAPFXXK4RAMRUXGY3POZSXE6JAMFXGIIDQOJXW24DUEBSGK5DFNZ2GS33OEBXWMIDFPB2HEYLUMVZHEZLTORZGSYLMEBWGSZTFEBXW4ICFMFZHI2BOEBKGQ2LTEBUXGIDGMFXHIYLTORUWGIDOMV3XGIDBNZSCA4DVORZSA5LTEBUW4IDQN5ZWS5DJN5XCAZTPOIQHK4ZAORXSAYTFM5UW4IDQNBQXGZJAOR3W6IDPMYQG65LSEBYGYYLOEBTG64RAO5XXE3DEEBSG63LJNZQXI2LPNYXCAV3FEB2W4ZDFOJZXIYLOMQQHS33VEBUGC5TFEBZXI33SMVSCA5DIMUQGY2LGMVTG64TNEBUW4IDUNBSSAYLHOJSWKZBAOVYG63RANRXWGYLUNFXW4IDBNZSCA43FOQQHI2DFEBWG6Y3LEB2G6IDVORUWY2L2MUQHI2DFEBYGC43TO5XXEZBAORXSAISTJFLEER2SPN3WQMC7NYZTGZDTL4ZTEX3CGRZTG435EIXCAV3FEB3WS3DMEBTG63DMN53SA5LQEBQWM5DFOIQGS3TWMVZXI2LHMF2GS3THEB2GQZJAOBZG65TJMRSWIIDMNFTGKZTPOJWSA53JORUCAZTVOJ2GQZLSEBUW443UOJ2WG5DJN5XHGLQKBJJWC3DVORQXI2LPNZZSYCSUOJUWC3THNRSSAQTPNFZQUKRBFIQSUIJKEEVCCKRBFIQSUIJKBJCU4RBAKRJECTSTJVEVGU2JJ5HAUKRBFIQSUIJKEEVCCKRBFIQSUIJK
```
```
decoded:
b'*!*!*!*!*!*!*!*!*\nINTERCEPTED ELECTRONIC MAIL COMMUNICATION\n*!*!*!*!*!*!*!*!*\nTO: brian.riggs@area51.cloud\nFROM: illuminati@secrets.us\nDATE: 5/14/2024@16:33\nSUBJ: RE: CONFIRMED EXTRATERRESTRIAL LIFE\nBODY:\nHello Brian,\n\nWe appreciate the information you have provided us regarding your discovery and prompt detention of extraterrestrial life on Earth. This is fantastic news and puts us in position for us to begin phase two of our plan for world domination. We understand you have stored the lifeform in the agreed upon location and set the lock to utilize the password to "SIVBGR{wh0_n33ds_32_b4s3s}". We will follow up after investigating the provided lifeform with further instructions.\n\nSalutations,\nTriangle Bois\n*!*!*!*!*!*!*!*!*\nEND TRANSMISSION\n*!*!*!*!*!*!*!*!*'
```
