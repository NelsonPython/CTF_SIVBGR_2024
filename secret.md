## Challenge:  Secret [Forensics] 150 Points
We managed to get an informant to obtain a document, Secret.pdf, which we believe might contain information confirming alien existence, but it is too censored.
## Solution
I used pdfminer to extract the flag:
```
$ sudo pip install pdfminer
$ pdf2txt.py Secret.pdf > Secret.txt
$ cat Secret.txt

Date: 08-16-1969

Extra-terrestrial life confirmed  :

After the quarantine period was completed ALIENT INVESTIGATION UNIT 
members Neil Armstrong Edwin Aldrin and Michael Collins were 
interrogated – confirming the presence of Extra-Terrestrial Life 
(ELF). 

Gen Stills orders are as follows:  Do not allow the population to 
learn of the ELFs. The astronauts involved are immediately to be 
briefed on the consequences of sharing information about these 
matters to the public. 

Additionally, the ALIENT INVESTIGATION UNIT has been granted an extra
300,000,000 for the continued investigation and containment of ELFs. 

At the moment, it is believed they intend no harm. The question of 
why they have traveled to Earth remains unanswered at the present 
time. As such, extreme caution will be taken. Any ELF spotted should 
be captured – failing that, it should be exterminated.

FLAG:  SIVBGR{C0nta1n_Th3_Al13ns}
```
