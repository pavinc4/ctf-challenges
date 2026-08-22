Room Link : [OhSINT](https://tryhackme.com/room/ohsint)

<img width="609" height="151" alt="Image" src="https://github.com/user-attachments/assets/19693ab1-8957-4d09-b906-8d9fd65bf57b" />

> `OSINT`
> 

---

# **Objective**

Finding possible information from the provided image using OSINT techniques.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d487283c-aae2-42bf-a2fa-ec13bb02d75b" />

---

## Initial Analysis

- The provided image appears to be the default Windows XP wallpaper, commonly known as “Bliss.”
- Since the image itself does not show any obvious information, the next step is to examine the image's **metadata** for possible clues.

---

## Checking Image Metadata

Using **ExifTool** to inspect the image metadata for any useful information or clues, we found details such as the **hash, location, and other metadata**.

<img width="1365" height="523" alt="Image" src="https://github.com/user-attachments/assets/1ecb7a87-08d9-47a5-aead-710e1f31ac42" />

We found out things like who owns the rights to the image, which **Copyright value** shows **`OWoodflint`**.
We searched for **`OWoodflint`** on Google and found the following links:

<img width="937" height="594" alt="Image" src="https://github.com/user-attachments/assets/3893eabb-83af-48e7-871f-378787f48e81" />

---
## What is this user's avatar of?
> `cat`
<img width="536" height="638" alt="Image" src="https://github.com/user-attachments/assets/ab27a2f7-698a-4d20-a6e2-028c0a92a492" />

---
## What city is this person in?
> `London`
<img width="956" height="667" alt="Image" src="https://github.com/user-attachments/assets/957a461d-aab4-4ef0-817b-51a3b2aced45" />

---
## What is the SSID of the WAP he connected to?
> `UnileverWiFi`
<img width="535" height="257" alt="Image" src="https://github.com/user-attachments/assets/a25ca502-71c1-4a3c-a3d1-d0a4c61d3982" />

- Using the BSSID from the tweet, we searched it on Wigle.net and found the SSID: `UnileverWiFi`. The location also shows London, matching the location mentioned on the GitHub profile and confirming it.

<img width="1021" height="369" alt="Image" src="https://github.com/user-attachments/assets/7e369a33-084d-45f6-96b1-c018b45b909e" />

---
## What is his personal email address?
> `OWoodflint@gmail.com`
<img width="957" height="653" alt="Image" src="https://github.com/user-attachments/assets/34d09930-e4de-468e-9f3f-6dea34fea7d5" />

---
## What site did you find his email address on?
> `Github`
<img width="966" height="767" alt="Image" src="https://github.com/user-attachments/assets/594d3844-2c17-415a-b8f4-7fc7560005c8" />

---

## Where has he gone on holiday?
> `New York`
<img width="1362" height="642" alt="Image" src="https://github.com/user-attachments/assets/081a25eb-70b1-41e9-b210-83bdffcbfb38" />

---

## What is the person's password?
> `pennYDr0pper.!`

- After finding nothing on Twitter or GitHub, we checked the WordPress blog and inspected its source code. We found a strange set of characters that looked like a password. It was hidden in white text, but using Ctrl+A to select all the text made it visible.
<img width="1365" height="659" alt="Image" src="https://github.com/user-attachments/assets/d76a98bf-883c-4899-b5d7-a3d0b0cba0b6" />

---
<h1 align="center">👍</h1>

