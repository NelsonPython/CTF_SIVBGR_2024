## Challenge:  You Have Mail [Forensics] 150 Points

This challenge is composed of an email, more specifically an .eml file. The email introduces the theme for the forensics group, which is a whistleblower announcing that alien life exists on Earth, and the government knows about it.

## Solution
### Step 1: View the .eml file
```
MIME-Version: 1.0
Date: Tue, 7 May 2024 22:12:36 -0500
Message-ID: <CAL1VkQ5JtBMJGOg+yuq6m51ETGDMj5nOHZswruxkD=yVRR9thQ@mail.gmail.com>
Subject: URGENT: Proof of UFO!!! Read in a secure location
From: Edward Silverfish <e.silverfish@uscg_ctf1.org>
To: media@uscgnews11.com
Content-Type: multipart/mixed; boundary="000000000000cd98100617e8acef"

--000000000000cd98100617e8acef
Content-Type: text/plain; charset="UTF-8"

Hello USGC News Channel media member,

My name is Dr. Edward Silverfish, and I am contacting you today to
provide you with information into what will be the next major news
story that will rock the headlines. I have evidence that the
government is actively hiding verifiable proof that alien life exists.
The government has a series of ongoing projects with the goal of
concealing this information from the public. I work within a lab in
Area 007, and I have seen with my own eyes what can only be described
as intelligent life that is not from this planet. I have worked on
unknown technology that can do things we have never seen before.
Things like Gen-AI that draws hands properly, bug spray that actually
stops all the bugs, and comms that deliver whale sounds straight from
the ocean to your bedroom! Just incredible stuff that our scientists
have never been able to replicate. This technology might be used to
develop breakthroughs, it if is just released to the public.

With your help, we can let the public know what is actually going on,
what type of life exists beyond Earth, and how to master Gen-AI art
once and for all.

Please see the attachment for my first piece of evidence. I am not
very good at understanding encryption details, but I did password
protect the file. The password is 53 65 63 75 72 65 5f 43 6f 64 65 3a
4f 72 64 65 72 5f 36 36.

Utilize a common number system to decrypt that password.

--000000000000cd98100617e8acef
Content-Type: application/zip; name="evidence.zip"
Content-Disposition: attachment; filename="evidence.zip"
Content-Transfer-Encoding: base64
X-Attachment-Id: f_lvx8qxe70
Content-ID: <f_lvx8qxe70>

UEsDBAoACQAAADewp1gfngtKRAAAADgAAAAMABwAZXZpZGVuY2UudHh0VVQJAAMZ6zpm6Oo6ZnV4
CwABBOgDAAAEAAAAADeIlKHufvfLJvJ/Ed32cRwF755eiG+bw1NAIL3UPKn+4WIMkSPXJInVFxLM
CrGuacbTdG6AcqrqzDiXWVhqKv6WuHlKUEsHCB+eC0pEAAAAOAAAAFBLAQIeAwoACQAAADewp1gf
ngtKRAAAADgAAAAMABgAAAAAAAEAAACkgQAAAABldmlkZW5jZS50eHRVVAUAAxnrOmZ1eAsAAQTo
AwAABAAAAABQSwUGAAAAAAEAAQBSAAAAmgAAAAAA
--000000000000cd98100617e8acef--
```

### Step 2:  The password is in hex format
I decoded it using https://cryptii.com/pipes/hex-decoder
```
Encoded:
53 65 63 75 72 65 5f 43 6f 64 65 3a 4f 72 64 65 72 5f 36 36

Decoded:
Secure_Code:Order_66
```
### Step 3:  The message is in Base64
Initially, I found newlines (\n) in the file when I copied it so I removed them to get one long variable:
```
UEsDBAoACQAAADewp1gfngtKRAAAADgAAAAMABwAZXZpZGVuY2UudHh0VVQJAAMZ6zpm6Oo6ZnV4CwABBOgDAAAEAAAAADeIlKHufvfLJvJ/Ed32cRwF755eiG+bw1NAIL3UPKn+4WIMkSPXJInVFxLMCrGuacbTdG6AcqrqzDiXWVhqKv6WuHlKUEsHCB+eC0pEAAAAOAAAAFBLAQIeAwoACQAAADewp1gfngtKRAAAADgAAAAMABgAAAAAAAEAAACkgQAAAABldmlkZW5jZS50eHRVVAUAAxnrOmZ1eAsAAQToAwAABAAAAABQSwUGAAAAAAEAAQBSAAAAmgAAAAAA
```
I noted that only upper and lower case characters so I used base64 to decode it and save the decoded values in a file called evidence.
```
echo UEsDBAoACQAAADewp1gfngtKRAAAADgAAAAMABwAZXZpZGVuY2UudHh0VVQJAAMZ6zpm6Oo6ZnV4CwABBOgDAAAEAAAAADeIlKHufvfLJvJ/Ed32cRwF755eiG+bw1NAIL3UPKn+4WIMkSPXJInVFxLMCrGuacbTdG6AcqrqzDiXWVhqKv6WuHlKUEsHCB+eC0pEAAAAOAAAAFBLAQIeAwoACQAAADewp1gfngtKRAAAADgAAAAMABgAAAAAAAEAAACkgQAAAABldmlkZW5jZS50eHRVVAUAAxnrOmZ1eAsAAQToAwAABAAAAABQSwUGAAAAAAEAAQBSAAAAmgAAAAAA
| Base64 -d > evidence
```

### Step 3: Determine the file type of evidence:
I used the file command to determine the file type:
```
└─$ file evidence      
evidence: Zip archive data, at least v1.0 to extract, compression method=store
```

### Step 4: Rename to evidence.zip and unzip it:
I used the password from the email:  Secure_Code:Order_66
```
└─$ unzip evidence.zip
```
### Step 5: Cat to view the contents:
Unzipping resulted in one file called evidence.txt.  I used cat to view it and found the flag.
```
└─$ cat evidence.txt
You found the evidence! 

 SIVBGR{th3_ev1d3nc3_1s_h3r3}
```

