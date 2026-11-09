# Linux Server Hardening Lab

I'm a Computer and Information Systems student and I'm interested in cybersecurity. I wanted to learn how to protect a Linux server by doing it myself, not just reading about it. So I built a small lab with two virtual machines. Ubuntu was the server I protected, and Kali Linux was the machine I used to attack it.

First I checked how secure Ubuntu was before I changed anything. Then I made it more secure and tried to break in from Kali to see if my changes worked.

The security score from Lynis went from 63 to 72.

## My lab

- MacBook Air running VirtualBox
- Ubuntu 26.04.1 at 10.0.2.15 (the server)
- Kali Linux at 10.0.2.3 (the attacker)
- Both machines are on their own private network, so nothing leaves the lab
- I connect to Ubuntu from my Mac with SSH

## What I did

**1. Checked the starting point.** I ran a tool called Lynis. It gave the server a score of 63 and said there was no firewall, no protection against login attacks, and no malware scanner.

**2. Turned on the firewall.** I used UFW to block all incoming connections except SSH.

**3. Made SSH safer.** I set up SSH keys for Kali and my Mac, then turned off password logins completely. I also blocked the root account from logging in, allowed only my user, and limited login attempts to 3. To test it, I tried to log in with a password from Kali and got "Permission denied (publickey)".

**4. Added fail2ban.** If someone fails to log in 3 times in 10 minutes, fail2ban blocks their IP address for 1 hour. My Mac is on an allow list so I can't lock myself out.

**5. Added a rootkit scanner.** rkhunter now scans the server every day. The first scan gave me 4 warnings. I checked each one before ignoring it, and all 4 turned out to be normal Ubuntu files and settings.

**6. Checked again.** I ran Lynis one more time and the score went up to 72.

## Attacking it from Kali

From Kali I kept trying to log in with a fake username. The first tries were denied. Then fail2ban blocked Kali, and after that the server would not even answer ("Connection refused").

![fail2ban blocking Kali](screenshots/fail2ban-ban.png)

![Firewall rule blocking Kali](screenshots/firewall-ban.png)

## Problems I ran into

- When I was copying my SSH key to Ubuntu, I kept typing my key passphrase instead of my Ubuntu password. I found the problem by reading the SSH logs.
- After I installed fail2ban, it looked like it wasn't working. The logs showed it was fine. I had just checked too fast.
- My first SSH login from my Mac got cut off. It was my own 30 second login time limit.
- I typed a zero instead of a capital O in a command and it failed. Small mistake, but it took a few minutes to find.

## Files

- `configs/` has my settings for SSH, fail2ban and rkhunter
- `evidence/` has the output that shows each part working, including the before and after Lynis reports
- `screenshots/` has the images above

## What's next

- Fix more of the Lynis suggestions to raise the score
- Write a script that can set up all of this on a new server automatically
- Send the server's logs to a monitoring tool so I don't have to check them by hand
