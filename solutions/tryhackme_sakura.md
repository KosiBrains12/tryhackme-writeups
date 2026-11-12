

# TryHackMe : Sakura Room : https://tryhackme.com/room/sakura

## Overview : This room shows various OSINT techniques and tools, how to use OSINT in real-life situations, and the consequences of oversharing online. This page covers how I tackled each problem. I won't include the answers, but you'll find plenty of useful tips for getting through each stage. Basic OSINT and Linux experience is recommended before starting.Let's dive in.

### This room simulates a dojo which were victims of a cyber attack

## Task 1: Finding the attacker's username

The first objective is to find the attacker's username. The attacker left behind an image, so I used it to pull as much information about them as possible.

I used [exif.tools](https://exif.tools), a site that reads the metadata embedded in image files. The room introduces it earlier on.

I ran the image through it and went through the results. My first pass was a quick skim and I missed it. On a second, slower look, the username was there in the metadata.

**Lesson:** read metadata slowly. The useful detail is often sitting in plain sight.

<img width="1919" height="875" alt="Screenshot 2026-11-11 050603" src="https://github.com/user-attachments/assets/4aebbcd8-c075-46fb-b267-77b9e9800dad" />

## Task 2: Finding the attacker's email address

The next objective is to find the attacker's email address. This one took me a while, because at the time I didn't know much about PGP (Pretty Good Privacy) keys. I didn't even guess that an email could be hidden in one.

PGP keys carry identity details, including an email address, inside them. After finding the attacker's PGP key in their repo, I opened it with Kleopatra, a tool for managing and reading PGP keys, and the email address was in the key's details.

**Lesson:** PGP keys hold more than just a key. If you ever see one during an investigation, check what's inside.

<img width="1849" height="805" alt="Screenshot 2026-11-11 052810" src="https://github.com/user-attachments/assets/344aaa9c-e33d-446b-8a86-6a2014c6042d" />

<img width="1909" height="777" alt="Screenshot 2026-11-11 053357" src="https://github.com/user-attachments/assets/a08c88c3-d654-4c72-8a08-a9f092fdf1eb" />

## Task 3: Finding the attacker's full name

The next objective is to find the attacker's full name. This one was quicker, since I already had the username from Task 1.

I searched for the username online and found an X (formerly Twitter) account belonging to the attacker. Their full name was on the profile.

To make sure, I searched the name itself and found a LinkedIn account matching it, which confirmed my suspicion.

**Lesson:** one username can lead to everything else. Reusing it across platforms makes someone easy to track, and checking a second source before trusting a lead is a good habit. 

<img width="885" height="777" alt="Screenshot 2026-11-11 125315" src="https://github.com/user-attachments/assets/2079b4e1-ffb7-4538-8e7a-8ab74506f8b0" />

## Task 4: Finding the cryptocurrency the attacker uses

The next objective is to find out which cryptocurrency the attacker uses. I went through the attacker's public repos, and the name of one of them hinted at the answer. I followed that lead and it turned out to be correct.


**Lesson:** people leave clues in the names of things they create. Repo names, usernames, and file names are all worth reading carefully.

<img width="1732" height="751" alt="image" src="https://github.com/user-attachments/assets/c6b8fe1e-a65c-4f56-ba93-1509e9eabdea" />

## Task 5: Finding the attacker's crypto wallet address

The next objective is to find the attacker's wallet address. I went straight to the repo from the previous task, but the wallet address wasn't in the current files.

The repo had two commits, which meant the attacker had edited the content at some point. I opened the commit history and looked at what the earlier commit contained. The wallet address was there.

**Lesson:** deleting something from a repo doesn't remove it from the history. Git keeps every previous version, so anything an attacker (or anyone) accidentally commits can still be found later.

<img width="1888" height="725" alt="image" src="https://github.com/user-attachments/assets/8f8080df-c149-4119-b672-2530fc9f27a4" />

## Task 6: Finding the mining pool that paid the attacker

The next objective is to find the mining pool the attacker received a payment from on January 23, 2021.

I used the wallet address from the previous task and searched it on [Etherscan](https://etherscan.io), a public blockchain explorer. It lists every transaction tied to an address, so I went through the history to the date in question and found the pool that sent the payment.

**Lesson:** blockchain transactions are public. If you can link a wallet to a person, you can see its entire payment history.

<img width="1783" height="961" alt="Screenshot 2026-11-11 153614" src="https://github.com/user-attachments/assets/a90ff304-48c8-469d-8223-0b73bbdd8430" />

## Task 7: Finding the other cryptocurrency the attacker exchanged

The last objective in this section is to find the other cryptocurrency the attacker exchanged using their wallet.

I went back to the Etherscan results from the previous task and looked at the wallet's transaction history. Most of the entries were hexadecimal addresses, but one showed up as a named token instead. which stood out, and it was the answer.

**Lesson:** when you're scanning a lot of similar-looking data, look for the thing that breaks the pattern.

<img width="1894" height="828" alt="Screenshot 2026-11-11 155417" src="https://github.com/user-attachments/assets/5ff8f25e-60d1-4a3e-8a31-e75724bb1d35" />

## Task 8: Finding the attacker's Twitter/X handle

The next objective is to find the attacker's Twitter/X handle. This one wasn't complicated.

I searched the attacker's full name on Google, and one of the results was an X account that matched. Searching the attacker's email address leads to the same account, so that's another way to get there.

**Lesson:** one piece of personal info (a name, an email) is enough to find a social media account. 

<img width="1917" height="786" alt="Screenshot 2026-11-11 160500" src="https://github.com/user-attachments/assets/2f7f1a76-fd97-40de-a531-d6a36bf5dc11" />

## Task 9: Finding the BSSID of the attacker's home WiFi

The next objective is to find the BSSID of the attacker's home WiFi network. This was probably the hardest task in the room, because I had to hunt for clues wherever I could find them.

The first clue was an image the attacker had posted online. It contained an MD5 hash.

<img width="669" height="177" alt="EsdhaUSVkAAM803" src="https://github.com/user-attachments/assets/6bfa00e4-8e05-4018-992b-10dcd974275b" />

The attacker posted: "Not too concerned about someone else finding them on the Dark Web. Anyone who wants them will have to do a real DEEP search to find where I PASTEd them."

I took this as a clue because DEEP and PASTE were the only words capitalized, and the MD5 hash alone wasn't leading anywhere. I looked up Deep Paste but didn't find anything useful. Things got more confusing, so I had to dig deeper, and I found out it's a pastebin site on the dark web where users anonymously paste text.

I went to Deep Paste and pasted the text the attacker posted, but the URL I got wasn't working. So I looked on Reddit and finally got a clue from someone: an image that contained the SSID of the attacker's home WiFi, which was DKIF-G.

<img width="1100" height="421" alt="1_7PHzjVEUx6PxTm6V-5z3iA" src="https://github.com/user-attachments/assets/54f34239-9e48-4b38-bd14-a80ab2196b2e" />

I searched the SSID on Wigle and was able to find info about the attacker's BSSID, i used that to answer the question

The next task is to find the airport closest to the location where the attacker shared a photo, before getting on their flight.


For this task, I looked up the image on X. In other words, I scanned it using Google (a reverse image search). I found the specific location it was taken at, and with that I was able to find the airports close to it.

<img width="1098" height="790" alt="Screenshot 2026-11-12 155713" src="https://github.com/user-attachments/assets/0ffcbb85-915c-4f42-b372-344b2a30a078" />

This was the image the attacker posted prior to getting on their flight

The next objective is to find the airport where the attacker had their last layover.

The next objective is to find the airport where the attacker had their last layover.

For this task, I looked at the attacker's Twitter account and found another photo they posted, which related to where they had their last layover. Taking a close look at the image, it was from the JAL first class lounge. I searched "JAL first class lounge" on Google and found the airline was Japan Airlines. The question asked for the specific airport though, so I had to check the airports Japan Airlines operates from. I tried the first two and they were incorrect, but I got it right on the third try.

<img width="443" height="523" alt="EsiM12KVoAEhAsI (1)" src="https://github.com/user-attachments/assets/bffc5aa5-0258-4167-8863-e8856b63a376" />

This was the image the attacker posted at their final layover

## Task 10: Finding the lake on the attacker's map

The last objective is to find the lake visible on the map the attacker shared during their final flight.

I used the map image from the attacker's tweet and ran it through Google's reverse image search. That showed me the map was of a region in Japan. I believed the lakes were the small, darker spots on the map. My first attempt didn't get me there, so I did an advanced search using wikipedia map and I got the answer on my second try.

<img width="1194" height="669" alt="EsiNRuRU0AEH32u" src="https://github.com/user-attachments/assets/42c06ee0-baa4-4801-b191-e42d49459bd2" />

## Final thoughts

The Sakura room took me a day to complete, and it was a fun experience regardless!







