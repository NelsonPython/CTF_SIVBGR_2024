## Challenge: Container Chronicles [Misc] 150 Points

Welcome, aspiring digital detectives, to the Container Chronicles challenge!
Your mission, should you choose to accept it, is to delve into the depths of a
seemingly ordinary image of a shipping container and uncover its hidden secrets.

Within the confines of this digital cargo hold lies a treasure trove of information
waiting to be unearthed. Your task revolves around deciphering the manufacturing date
of the shipping container concealed within the image. But beware, for the information
you seek is not readily visible to the naked eye.

Utilizing your skills, you must meticulously examine every pixel, every byte, and every
hidden layer within the image to extract the elusive manufacturing date. Employ various
techniques, ranging from simple visual inspection to advanced data analysis, to uncover the
truth lurking beneath the surface.

Remember, in the world of digital forensics, every detail matters. Stay vigilant, think
outside the box, and be prepared to embark on a journey through the intricate labyrinth of data
concealed within the confines of this innocuous image.

Are you ready to unravel the mysteries of the Container Chronicles? The fate of the cargo
rests in your hands. Good luck, and may the digital winds guide you to victory!

## Solution
I used aperisolve.com to find part of the flag that was hidden in the RGB red channel:
    
SIVBGR{MM_DD_YYYY_d0ck3r}
MANUFACTUREDATE

I read the clue and tried the manufacture date of Docker, but that did not work.  I was very happy to find a writeup by pbchocolate on github:
[pbchocolate writeup for Container Chronicles](https://github.com/sa1181405/uscybergames-pbchocolate-writeups/blob/main/uscybergames/beginner-room/misc/Container%20Chronicles.md)

In this writeup, I did not understand the sentence:

"Reversing the image, we get this site:
http://intermodalnetwork.blogspot.com/2011/12/nyk-line-container-nyku-257454-2-high.html"

But it gave me the idea to use Google Image Search to find more information about the shipping container.

```
https://lens.google.com/search?ep=gisbubb&hl=en&re=df&p=AbrfA8oJTZukxw0ISex31lZ-ni77qW4zDxGKF_O1hSOsst5WtgPQSTZNMJYB0dSeMacx3iNX1vsaMP56ALmXCvlZssJIIXqNuFuW77AnMe4p9QezfdIYa8xWXis3zbqXJ2ZWwf_tlXICgDpPfALut2k8MTNkEp5qB4WjiGxMYswcQLHoTO8dtnjDLr_n247lxYPMc6BbSIj-fs7H3A%3D%3D#lns=W251bGwsbnVsbCxudWxsLG51bGwsbnVsbCxudWxsLDEsIkVrY0tKREUwWTJVMk5UazRMVEV3TnpNdE5HWXpaUzFoTkRsbExUZzRPV0kzWlRObE4yUmpNaElmU1RGSGJHOVVUbkJIVkZGaWMwNXBYM3AxVm1GWlRWaGliVlozT1VGU2F3PT0iLG51bGwsbnVsbCxbW251bGwsbnVsbCwiaXAtMCJdLFsiZDkwZTI2YzgtNmM2Yi00N2FhLWEyYTUtYzVjYjY3NGQyMmUyIl1dLG51bGwsbnVsbCxudWxsLFtudWxsLG51bGwsW251bGwsWzAsMCwxMDAwMDAsMTAwMDAwXV1dLFsiZDkwZTI2YzgtNmM2Yi00N2FhLWEyYTUtYzVjYjY3NGQyMmUyIl1d
```

The first entry of the Google result set was the same link as in the writeup: 

```
http://intermodalnetwork.blogspot.com/2011/12/nyk-line-container-nyku-257454-2-high.html
```

I googled "40' high-cube Seacube" and entered "DRYU9290583" to find the manufacture date. 

### SIVBGR{02_10_2011_d0ck3r}








