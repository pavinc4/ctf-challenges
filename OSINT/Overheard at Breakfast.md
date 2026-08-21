Room Link : [Overheard at Breakfast](https://tryhackme.com/room/hh-overheardatbreakfast-6f01793c)

<img width="650" height="148" alt="Image" src="https://github.com/user-attachments/assets/4159c8b0-6ab7-4089-b538-3afe032d864e" />

> `OSINT` , `Cryptography`
> 

---

# **Objective**

Analyze the captured screenshot evidence to extract leaked information, pivot to the target's hidden profile and decode the final flag hidden in the profile's backend data.

> Step 1 - Finding the Clues
> 

> Step 2 - Tracking Down the Profile
> 

> Step 3 - Finding and Decoding the Secret Text
> 

---

## The Scenario :

A guest at the fictional Byte Lotus Hotel overhears a conversation at the breakfast terrace between two strangers “Ponzi,” a self-described influencer, and “Lambo,” someone who claims to have gone quiet on social media. Before the conversation disappears, our guest grabs a screenshot.

---

## Step 1 : Finding the Clues

<img width="1175" height="781" alt="Image" src="https://github.com/user-attachments/assets/2a400876-099b-4828-9d55-cae4bda68bc3" />

Two details stood out immediately:

1. A **free tool “starting with G”** :
    - They mentioned a free tool that allows users to upload a profile picture and link their other social media accounts. Since the message is focused on **managing and connecting social media profiles**, this made “**Gravatar** (Globally recognized Avatar)” a likely possibility.
2. An email address :
    - An email address was also shared openly. Gravatar uses an email address to associate it with a public profile containing an avatar, bio, and links to other social accounts.

---

## Step 2 : Tracking Down the Profile

Critically, Gravatar doesn’t let you look someone up by typing their raw email into a URL. Instead, it identifies profiles using a **hash of the email address.**

- We generated the hash of the email address using CyberChef and then entered the same email address into Gravatar’s Email Checker tool. It returned the same SHA-256 hash that we obtained from CyberChef, along with the associated profile and avatar URLs.

<img width="1152" height="309" alt="Image" src="https://github.com/user-attachments/assets/9713d847-7f5c-49a8-92da-cf62e8550b4b" />

The matching hash was:

`d43faafe9d7f056793bd037b8d6e321acad985c222d83775b10d6539e301e931`

---
## Step 3 : Finding and Decoding the Secret Text

- Opening the profile URL takes us to the Gravatar profile, which is named **“Lambo.”**
<img width="664" height="459" alt="Image" src="https://github.com/user-attachments/assets/336418a1-54e1-4d55-b34f-76ec1348a871" />

The string sitting in the description field was a Base64-encoded blob 

At a glance, this isn’t a hash. Hashes like MD5/SHA-256 are fixed-length **hexadecimal** strings (only `0-9` and `a-f`). This string has mixed-case letters, numbers, and no obvious fixed pattern matching hex  a strong sign of **Base64 encoding**, which is reversible rather than one-way.

<img width="888" height="445" alt="Image" src="https://github.com/user-attachments/assets/7fff8d40-1640-4a5c-9447-2ea289ddb18f" />

Running it through CyberChef’s `From Base64` operation reveals a clean, readable flag in the classic `THM{...}` format.

---
<h1 align="center">👍</h1>

