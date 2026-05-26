Room Link : [Juicy Details](https://tryhackme.com/room/juicydetails)

<img width="769" height="150" alt="Image" src="https://github.com/user-attachments/assets/784144a5-bf73-473e-ad66-19925c5c1db3" />


> `forensics` , `log analysis` , `web` 

---

# **Objective**

The goal of this challenge was to analyze compromised server logs (access.log, auth.log, vsftpd.log) to  investigating the attack to determine what techniques and tools the attacker used, what endpoints were vulnerable, and what sensitive data was accessed and stolen.

---

## TASK: **Reconnaissance**



### What tools did the attacker use? (Order by the occurrence in the log)

<img width="1375" height="882" alt="Image" src="https://github.com/user-attachments/assets/be1e8af8-6c6d-4abd-95cf-988ceba42bcf" />

- **Nmap** : The initial network scanning phase, indicated by Nmap Scripting Engine in the User-Agent.
- **Hydra** : An automated brute-force campaign targeting the user login mechanism, signed by Mozilla/5.0 (Hydra).
- **SQLmap** : An automated SQL injection tool targeting the product search endpoint, logged directly as sqlmap/1.5.2#stable.
- **Curl** : A single command-line web request tool used manually or via custom scripts, identified by curl/7.74.0.
- **Feroxbuster** : A high-speed directory brute-forcing tool utilized to discover hidden pathways, identified by feroxbuster/2.2.1.

> Answer: nmap, hydra, sqlmap, curl, feroxbuster

---

### What endpoint was vulnerable to a brute-force attack?

<img width="1374" height="302" alt="Image" src="https://github.com/user-attachments/assets/ac66afba-d8a2-4bfd-9d0d-83bc5a474a7b" />


> Answer: /rest/user/login

---

### What endpoint was vulnerable to SQL injection?

<img width="1358" height="395" alt="Image" src="https://github.com/user-attachments/assets/d144f03d-873c-4845-af48-b3c8a93119dd" />


>  Answer: /rest/products/search

---

### What parameter was used for the SQL injection?

<img width="1364" height="372" alt="Image" src="https://github.com/user-attachments/assets/2d103da0-2560-414b-9bc4-b90f75084c22" />

-  In web architecture, everything following the question mark (?) passes variables to the server. The letter q represents the query parameter box used by the search interface.
-  The screenshot confirms that the automated extraction tool (sqlmap) appended database commands directly into the q= value fields.
-  Because the application processed input inside the q parameter directly into database queries without validation, the database executed the injected logical operators, confirming q as the entry vector.
  
> Answer: q

---

### What endpoint did the attacker try to use to retrieve files? (Include the /)

<img width="1375" height="399" alt="Image" src="https://github.com/user-attachments/assets/6c3945b3-8208-4a73-8a05-a4f448c3a5b5" />

> Answer: /ftp

---

## TASK : STOLEN DATA

---

### What section of the website did the attacker use to scrape user email addresses?

<img width="1375" height="421" alt="Image" src="https://github.com/user-attachments/assets/f7af4128-8dba-4532-ac72-163e9ada90f0" />

- The logs show an automated script moving step by step through different product numbers like `/1/`, `/24/`, `/6/`, and `/42/` to access the review path.
- The backend application has a flaw where it includes private profile details like cleartext email addresses inside the public data sent for reviews.
- The attacker used quick, automated requests to read every product ID and pull out the hidden email addresses attached to the public review listings.
  
> Answer: product reviews

---

### Was their brute-force attack successful? If so, what is the timestamp of the successful login? (Yay/Nay, 11/Apr/2021:09:xx:xx +0000)

<img width="1326" height="331" alt="Image" src="https://github.com/user-attachments/assets/de1a79ef-9f6e-4fd3-bae2-69c36965bc95" />

-  Amidst many failed login attempts returning 401 and 500 errors, one single line returns an HTTP 200 OK status code. This means the server accepted the correct login password.
-  The failed requests only return 26 bytes of data because they show a short error message. The successful request returns 831 bytes of data because the server is handing over a full user login session token to the attacker.

> Answer: Yay, 11/Apr/2021:09:16:31 +0000

---

### What user information was the attacker able to retrieve from the endpoint vulnerable to SQL injection?

<img width="1364" height="145" alt="Image" src="https://github.com/user-attachments/assets/00d09f81-b2f1-4eb4-add2-431e7b5d9c68" />

- The attacker used a `UNION SELECT` statement injected directly into the search query to force the application database to combine secret tables with public search results.
- The highlighted fields prove the attacker explicitly called for the `id`, `email`, and `password` columns from the Users database table.
- The server responded with an HTTP 200 OK code, meaning it processed the command and dumped the database usernames and corresponding login passwords directly back to the attacker's system.
  
> Answer: email, password

---

### What files did they try to download from the vulnerable endpoint? (endpoint from the previous task, question #5)

<img width="1370" height="170" alt="Image" src="https://github.com/user-attachments/assets/768d9afb-1c45-487c-abcc-b8b4a4f0f6f2" />

- Immediately after discovering the `/ftp` folder, the attacker manually used a browser to guess specific files inside that folder directory.
- The highlighted log entries show requests for files ending in the `.bak` extension, which indicates system backup files containing website code or configuration data.
- The server returned an HTTP 403 Forbidden code for both attempts. This proves the web server interface blocked the attacker from reading or downloading these files over standard web traffic.

> Answer: coupons_2013.md.bak, www-data.bak

---

### What service and account name were used to retrieve files from the previous question? (service, username)

<img width="1211" height="166" alt="Image" src="https://github.com/user-attachments/assets/4de8a97e-3fc9-4971-ad74-c82ef4760478" />

- The log file comes from the `vsftpd` system service. The highlighted indicator inside the square brackets explicitly shows [ftp] as the protocol service handling the connection.
- The highlighted connection log at `09:35:37` shows a successful session authentication labeled as `OK LOGIN`. The use of a simple question mark `anon password "?" ` verifies that the attacker used the default unauthenticated `anonymous` guest account alias to pass through system security.
- Unlike the web requests that were blocked with a 403 error, this backend protocol access allowed the attacker to completely bypass web restrictions and successfully execute an `OK DOWNLOAD` for both backup files.

> Answer: ftp, anonymous

---

### What service and username were used to gain shell access to the server? (service, username)

<img width="935" height="94" alt="Image" src="https://github.com/user-attachments/assets/d6f678cb-24a1-40f0-adb6-894c96000ec9" />

-  The command logs filter out the system `auth.log` entries specifically for the `sshd` process. This explicitly confirms that Secure Shell (ssh) was the target daemon service vector.
-  The filtered terminal lines display an explicit message stating `Accepted password for www-data`. This proves the attacker successfully authenticated and logged directly into the system configuration under the `www-data` service username account

Answer: ssh, www-data

---

<h1 align="center">👍</h1>
