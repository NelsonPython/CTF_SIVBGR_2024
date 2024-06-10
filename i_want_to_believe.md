## Challenge:  I Want to Believe [Forensics] 150 Points

We've received a GIFt from what appears to be a signal coming from extraterrestrial life! Although, it appears they've used steganography to hide it inside of this .gif file. All we know is that it's in the form of a text file named 'iwanttobelieve.txt'. Can you recover it?

## Solution
The instructions contain 2 important clues.  First, "We received a GIFt".  I searched for a steganography tool called gift and found GIFT at:

https://github.com/dtmsecurity/gift?ref=dtm.uk

I downloaded the scripts and read the instructions.  The second clue gives the filename:  All we know is that it's in the form of a text file named 'iwanttobelieve.txt'

I ran this command:
```
└─$ python3 gift-cli.py --source /home/kali/06_images/gifs/gift.gif --dest out_gift recover iwanttobelieve.txt
```
```
└─$ cat iwanttobelieve.txt 

HELLO HUMANS. WE COME IN PEACE.

MY NAME IS J0K3 AND I AM BROADCASTING THIS MESSAGE FROM SIGMA CENTAVRI.

WE FORMALLY APOLOGIZE FOR ABDUCTING SO MANY OF YOUR KIND. AND ALSO THE COWS.

WE HOPE YOU ACCEPT THIS TOKEN OF ATONEMENT.

OUR RESEARCH SHOWS IT IS HIGHLY PRIZED BY YOUR KIND.
 ___________________________
< SIVBGR{y0ur_g1ft_1s_h3r3} >
 ---------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

PS: THE HUMANS WERE DROPPED OFF IN BORNEO.
ALSO, WE ARE KEEPING THE COWS. I NAMED THIS ONE "ANTHONY".
```
