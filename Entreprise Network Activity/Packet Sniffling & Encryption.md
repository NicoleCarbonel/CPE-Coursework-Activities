# CMPE 361 — Enterprise Networking
## Exercise: Network Security (Packet Sniffing, Encryption & Hashing)

This repository documents a lab exercise exploring network packet sniffing with Wireshark, classical encryption with the Vigenère cipher, and cryptographic hashing (MD5/MD4).

> **Note on privacy:** Personal identifiers such as the author's public IP address and NIC MAC address have been redacted or replaced with placeholder values. The credentials shown (`CPEUSER` / `PASSWORD`) belong to a public training/test site ([vbsca.ca](http://vbsca.ca)) intentionally built for teaching packet sniffing — they are not a real compromised account.

---

## I. Objectives

By the end of this exercise, the goal was to:
- Familiarize with packet sniffing tools and encryption methods
- Explain the information that can be sniffed by a packet analyzer
- Explain the fields of a captured packet
- Explain how to encrypt and decrypt data
- Explain the importance of encryption and hashing

## II. Tools Used

- Wireshark (network packet analyzer)
- Vigenère Cipher Table
- Hashcalc
- Online tools: [cryptii.com](https://cryptii.com/pipes/vigenere-cipher) (Vigenère), online MD5/MD4 generator

---

## III. Part 1 — Packet Sniffing

### A. IP Addressing

| Address Type | Value |
|---|---|
| Local IP Address (`ipconfig`) | `192.168.x.x` *(private/redacted)* |
| MAC Address | `XX:XX:XX:XX:XX:XX` *(redacted)* |
| Public IP Address (ipchicken.com) | *(redacted)* |

Public IP addresses of sample websites (safe to publish — these are public-facing servers, not personal data):

| Website | Public IP |
|---|---|
| www.pup.edu.ph | 155.137.80.132 |
| emabini.pup.edu.ph | 18.143.226.38 |
| sisstudents.pup.edu.ph | 113.19.104.67 |
| google.com | 142.250.197.46 |
| ched.gov.ph | 172.64.148.90 |
| facebook.com | 157.240.211.35 |
| messenger.com | 57.144.64.141 |
| youtube.com | 142.250.198.142 |

### B. Wireshark Packet Capture

Logged in to a test login page (`vbsca.ca`) using dummy credentials while capturing traffic, then filtered on `http.request.method == "POST"` to isolate the login request.

| Field | Value |
|---|---|
| No. of bits | 6064 bits |
| Source MAC Address | *(redacted)* |
| Destination MAC Address | `a8:f5:ac:40:c1:a0` |
| Type | IPv4 |
| Source IP Address | *(redacted)* |
| Destination IP Address | 23.155.129.172 |
| Header Length | 20 bytes |
| Source Port Number | 49921 |
| Destination Port Number | 80 |
| Flags | 0x018 (PSH, ACK) |
| Username | CPEUSER |
| Password | PASSWORD |

---

## IV. Part 2 — Encryption

### A. Vigenère Cipher — Encryption

| Plaintext | CYBERSECURITY |
|---|---|
| Keyword | JORLYJORLYJOR |
| Ciphertext | LMSPPBSTFPRHP |

### B. Vigenère Cipher — Decryption

| Ciphertext | SVEBZXWGVOII |
|---|---|
| Keyword | SECURESECURE |
| Plaintext | ARCHITECTURE |

### C. Verifying with an Online Vigenère Tool

Checked results against [cryptii.com](https://cryptii.com/pipes/vigenere-cipher) — outputs matched.

- Encrypt `WIRELESS` with keyword `LOCAL` → **HWTFXPDS**
- Decrypt `RRVNMGDVSG` with keyword `NETWORK` → **ELECTRONIC**

---

## V. Part 3 — Hashing

Hashed the string `CYBERSECURITY` using both Hashcalc (local tool) and an online MD5/MD4 generator.

| Algorithm | Hash Value |
|---|---|
| MD5 | `13264024CA874CBAFA7BC77BB28FE6B7` |
| MD4 | `1F314C78DE9CA01DF432A33CA75A6C4D` |

Both tools produced identical hashes for the same input.

---

## VI. Questions and Answers

**1. Why is the IP address from the command prompt different from the ipchicken IP address?**

`ipconfig` shows your *private/local* IP address, assigned by your router via NAT for use inside your home or school network. ipchicken.com shows your *public* IP address — the one your ISP assigns to your router, which is what the rest of the internet actually sees. Multiple devices on the same private network can share one public IP.

**2. Are the username and password displayed? Why?**

Yes. The test login page sends the form data over plain HTTP instead of HTTPS, so the POST request isn't encrypted. Wireshark can capture the raw packet and read the username and password fields in plaintext — which is exactly why sites handling real credentials must use HTTPS/TLS.

**3. Can you determine the manufacturer of the NIC? What is the manufacturer of your NIC?**

Yes — the first three octets of a MAC address (the OUI, Organizationally Unique Identifier) are registered to a specific manufacturer and can be looked up in public OUI databases (e.g., IEEE's registry or sites like macvendors.com).

**4. Can you encrypt and decrypt if you do not have the keyword?**

Not directly — the Vigenère cipher requires the keyword to shift each letter correctly. However, it isn't unbreakable: techniques like Kasiski examination or frequency analysis can sometimes recover the key length and content, especially with longer ciphertext or short/repeating keywords. This is part of why classical ciphers like Vigenère are considered weak by modern standards.

**5. What can you say about hashing even though it uses different applications?**

Hashing is deterministic — the same input run through the same algorithm (MD5, MD4, etc.) always produces the same output, regardless of which tool or platform computes it. That consistency is what made Hashcalc and the online hash tool return identical results, and it's what makes hashes useful for verifying data integrity.

---

## VII. Conclusion

This exercise showed how vulnerable unencrypted network traffic is to interception — tools like Wireshark can easily expose credentials sent over plain HTTP, reinforcing the importance of HTTPS for any sensitive data. Working with the Vigenère cipher demonstrated basic principles of encryption and decryption, but also its limitations as a classical, breakable cipher compared to modern algorithms. Finally, the hashing activity confirmed that cryptographic hash functions are deterministic and consistent across tools, making them reliable for verifying data integrity rather than for encryption or confidentiality, since hashes aren't meant to be reversed.

---

*CMPE 361: Enterprise Networking — Exercise, Network Security*
