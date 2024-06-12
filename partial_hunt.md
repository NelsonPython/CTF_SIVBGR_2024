## Challenge:  Hunt [Web] 150 Points

Agent, it looks like ARIA has spun up a simple website.  Is there anything you can find to give more information about it's plans?

https://uscybercombine-s4-hunt.chals.io/

## Solution:

Part of the flag and a clue can be found by viewing the source of the main page:

```
  <!-- Don't forget to check in on the bots! -->
    <!-- p1: SIVBGR{r1s3_ -->
```

In addition to the three images of robots on the main webpage, I found another, robot.jpeg, in /static/img/robot.jpeg

A few days after the US Cyber Open, I found this write-up on Medium:
  US Cyber Open 2024 — Challenges Writeup
  by Guy Ben-Yishai
  [https://medium.com/@guyby/us-cyber-open-2024-challenges-writeup-c0761efacb8d](https://medium.com/@guyby/us-cyber-open-2024-challenges-writeup-c0761efacb8d)

Using the hints provided, I completed the challenge by finding the /robots.txt folder:

[https://uscybercombine-s4-hunt.chals.io/robots.txt](https://uscybercombine-s4-hunt.chals.io/robots.txt)

I navigated to the secret-bot-spot and found the robot.js script:
view-source:https://uscybercombine-s4-hunt.chals.io/secret-bot-spot
view-source:https://uscybercombine-s4-hunt.chals.io/static/js/robot.js

### SIVBGR{r1s3_0f_th3_r0b0ts!}




