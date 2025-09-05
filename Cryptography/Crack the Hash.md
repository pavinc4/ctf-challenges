
<div align="center" style="display:inline-block;">
<img width="483" height="153" alt="Image" src="https://github.com/user-attachments/assets/41479330-588d-4639-af50-a5f5f6896542" /><br>
  Room Link: <a href="https://tryhackme.com/room/crackthehash">Crack the Hash</a>
</div><br>



---

This room is all about **identifying and cracking hashes**.  

The main steps followed:

- **Identify hash type** → using `hashid` and `Hash-Analyzer`.  
- **Crack hashes** → with `hashcat` and the `rockyou.txt` wordlist.  
- **Fallback checks** → using online tools like CrackStation for verification. 

---

<div align="center">
  <h1>Level 1</h1>
</div>



<div align="center">
  <h3> 1. 48bb6e862e54f2a795ffc4e541caed4d</h3>
</div><br><br>

## Identify | hashid


```
┌──(kali㉿kali)-[~]
└─$ hash-identifier
--------------------------------------------------
 HASH: 48bb6e862e54f2a795ffc4e541caed4d

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))

```
Hash Type : `MD5`


## Crack | hashcat

```
┌──(kali㉿kali)-[~]
└─$ hashcat -m 0 hash.txt rockyou.txt
hashcat (v6.2.6) starting

48bb6e862e54f2a795ffc4e541caed4d:easy

Session..........: hashcat  
Status...........: Cracked  
Hash.Mode........: 0 (MD5)  
Hash.Target......: 48bb6e862e54f2a795ffc4e541caed4d


┌──(kali㉿kali)-[~]
└─$ hashcat -m 0 hash.txt --show          

48bb6e862e54f2a795ffc4e541caed4d:easy

```

## Crackstation

<img width="1048" height="95" alt="Image" src="https://github.com/user-attachments/assets/c6f653e8-d872-466f-9cac-b6372484f36a" />
<br>

Cracked Password: `easy`

---




<div align="center">
  <h3> 2. CBFDAC6008F9CAB4083784CBD1874F76618D2A97</h3>
</div><br><br>

## Identify | hashid

```
┌──(kali㉿kali)-[~]
└─$ hash-identifier             
--------------------------------------------------
 HASH: CBFDAC6008F9CAB4083784CBD1874F76618D2A97

Possible Hashs:
[+] SHA-1
[+] MySQL5 - SHA-1(SHA-1($pass))

```
Hash Type : `SHA-1`


## Crack | hashcat

```
┌──(kali㉿kali)-[~]
└─$ hashcat -m 0 hash.txt rockyou.txt
hashcat (v6.2.6) starting

cbfdac6008f9cab4083784cbd1874f76618d2a97:password123      
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 100 (SHA1)
Hash.Target......: cbfdac6008f9cab4083784cbd1874f76618d2a97


┌──(kali㉿kali)-[~]
└─$ hashcat -m 100 hash.txt --show      
cbfdac6008f9cab4083784cbd1874f76618d2a97:password123


```

## Crackstation

<img width="1048" height="95" alt="Image" src="https://github.com/user-attachments/assets/34f8c391-93dc-4ab7-926f-10eba2f8752f" />
<br>

Cracked Password: `password123`

---




<div align="center">
  <h3> 3. 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032</h3>
</div><br><br>


## Identify | hashid
```
┌──(kali㉿kali)-[~]
└─$ hash-identifier
--------------------------------------------------
 HASH: 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032

Possible Hashs:
[+] SHA-256
[+] Haval-256

```
Hash Type : `SHA-256`

## Crack | hashcat

```
┌──(kali㉿kali)-[~]
└─$ hashcat -m 1400 hash.txt rockyou.txt 
hashcat (v6.2.6) starting

1c8bfe8f801d79745c4631d09fff36c82aa37fc4cce4fc946683d7b336b63032:letmein
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1400 (SHA2-256)
Hash.Target......: 1c8bfe8f801d79745c4631d09fff36c82aa37fc4cce4fc94668...b63032


┌──(kali㉿kali)-[~]
└─$ hashcat -m 1400 hash.txt --show     
1c8bfe8f801d79745c4631d09fff36c82aa37fc4cce4fc946683d7b336b63032:letmein

```
## Crackstation

<img width="1048" height="95" alt="Image" src="https://github.com/user-attachments/assets/136b890a-1fda-4abd-b0c0-3d7fec863456" />
<br>

Cracked Password: `letmein`


---

<div align="center">
  <h3> 4. $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom</h3>
</div><br><br>


## Identify | Hash Analyzer

<img width="784" height="145" alt="Image" src="https://github.com/user-attachments/assets/0159da66-ba9e-411f-8a26-5e67a778c10c" />


Hash Type : `bcrypt`

- This type of hash can take a very long time to crack,
  so either filter rockyou for four character words, or use a mask for four lower case alphabetical characters.

## Crack | hashcat

```
┌──(kali㉿kali)-[~]
└─$ hashcat -m 3200 hash.txt shortlist.txt 
hashcat (v6.2.6) starting

$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom:bleh

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 3200 (bcrypt $2*$, Blowfish (Unix))
Hash.Target......: $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX...8wsRom

┌──(kali㉿kali)-[~]
└─$ hashcat -m 3200 hash.txt --show
$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom:bleh
```

Cracked Password: `bleh`

---


<div align="center">
  <h3> 5. 279412f945939ba78ce0758d3fd83daa</h3>
</div><br><br>


## Identify | hashid
```
┌──(kali㉿kali)-[~]
└─$ hash-identifier                
--------------------------------------------------
 HASH: 279412f945939ba78ce0758d3fd83daa

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))

```
Hash Type : `MD5`


## Crack | hashcat

```
┌──(kali㉿kali)-[~]
└─$ hashcat -m 900 hash.txt rockyou.txt
hashcat (v6.2.6) starting

279412f945939ba78ce0758d3fd83daa:Eternity22
```

## Crackstation

<img width="1048" height="95" alt="Image" src="https://github.com/user-attachments/assets/54753e8c-b9e0-4e57-8a0c-ba9b3ba8c9da" />
<br>

Cracked Password: `Eternity22`











---

<div align="center">
  <h1>Level 2</h1>
</div>

<div align="center">
  <h3> 1. F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85</h3>
</div><br><br>

## Identify | hashid

```
┌──(kali㉿kali)-[~]
└─$ hash-identifier                
--------------------------------------------------
 HASH: F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85

Possible Hashs:
[+] SHA-256
[+] Haval-256

```
Hash type : `SHA-256`

## Crack | hashcat

```
┌──(kali㉿kali)-[~]
└─$ hashcat -m 1400 hash.txt rockyou.txt 
hashcat (v6.2.6) starting

f09edcb1fcefc6dfb23dc3505a882655ff77375ed8aa2d1c13f640fccc2d0c85:paule
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1400 (SHA2-256)
Hash.Target......: f09edcb1fcefc6dfb23dc3505a882655ff77375ed8aa2d1c13f...2d0c85


┌──(kali㉿kali)-[~]
└─$ hashcat -m 1400 hash.txt --show     
f09edcb1fcefc6dfb23dc3505a882655ff77375ed8aa2d1c13f640fccc2d0c85:paule
```

## Crackstation

<img width="1048" height="95" alt="Image" src="https://github.com/user-attachments/assets/3768d45f-451a-4252-b6f5-4a497ea24e4e" />


Cracked Password: `paule`

---

<div align="center">
  <h3> 2. 1DFECA0C002AE40B8619ECF94819CC1B</h3>
</div><br><br>

## Identify | hashid

```
┌──(kali㉿kali)-[~]
└─$ hash-identifier                     
--------------------------------------------------
 HASH: 1DFECA0C002AE40B8619ECF94819CC1B

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))

Least Possible Hashs:
[+] RAdmin v2.x
[+] NTLM


```
Hash type `NTLM`

## Crack | hashcat

```
┌──(kali㉿kali)-[~]
└─$ hashcat -m 1000 hash.txt rockyou.txt 
hashcat (v6.2.6) starting

1dfeca0c002ae40b8619ecf94819cc1b:n63umy8lkf4i             
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1000 (NTLM)
Hash.Target......: 1dfeca0c002ae40b8619ecf94819cc1b
```
## Crackstation

<img width="1048" height="95" alt="Image" src="https://github.com/user-attachments/assets/eb841946-9215-443f-a019-3e71d436baf8" />

Cracked Password: `n63umy8lkf4i`

---


<div align="center">
  <h3> 3. $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.</h3>
</div><br><br>

```
HASH INFO :

Hash: $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.
Salt: aReallyHardSalt
```

## Identify | hashes.com

<img width="1734" height="106" alt="Image" src="https://github.com/user-attachments/assets/c7ceb894-e250-41d9-a513-6305edfc83da" />

Hash type `SHA512crypt`

## Crack | hashcat

```
┌──(kali㉿kali)-[~]
└─$ hashcat -m 1800 hash.txt rockyou.txt 
hashcat (v6.2.6) starting

$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.:waka99
```

Cracked Password: `waka99`

---


<div align="center">
  <h3> 4. e5d8870e5bdd26602cab8dbe07a942c8669e56d6</h3>
</div><br><br>

```
HASH INFO :

Hash: e5d8870e5bdd26602cab8dbe07a942c8669e56d6
Salt: tryhackme
```

## Identify | hasid

```
 HASH: e5d8870e5bdd26602cab8dbe07a942c8669e56d6

Possible Hashs:
[+]  SHA-1
[+]  MySQL5 - SHA-1(SHA-1($pass))
```
Hash type `sha1($pass.$salt)`

## Crack | hashcat

```
┌──(kali㉿kali)-[~]
└─$ hashcat -m 110 hash.txt rockyou.txt 
hashcat (v6.2.6) starting

e5d8870e5bdd26602cab8dbe07a942c8669e56d6:481616481616
```

Cracked Password: `481616481616`

---



## Conclusion

This room is covered:   

- **Level 1**: Basic hash recognition & simple cracking.
- **Level 2**: Complex salted hash cracking .

Takeaway: Identify the hash type, choose the right mode, and crack efficiently.
