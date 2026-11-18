# 🔓 Password Cracking with JTR & NetworkWalks Tools


**W3-PM1 & W3-PM2 | Cybersecurity | Networkwalks**

This repository documents my Week 3 project work: **Phase 3 of a pentest, Gaining Access**, through password cracking on password-protected PDF files provided by NetworkWalks as training lab material. I tested three different locked PDFs across multiple tools and platforms.

> ⚠️ All files used here are lab files provided by Network Walks for training purposes. No real-world targets, systems, or data were involved.

---

## 🗺️ Where this fits in a pentest

| # | Phase | Status |
|---|---|---|
| 1 | Reconnaissance & Footprinting | ✅ Covered in Week 2 |
| 2 | Scanning & Enumeration | ✅ Covered in Week 2 |
| 3 | Gaining Access | ✅ Covered in this report (password cracking) |
| 4 | Maintaining Access | ⏳ Upcoming |
| 5 | Covering Tracks & Reporting | ✅ Covered in Week 2 |

---

## 🧰 Tools Used

| Tool | Purpose |
|---|---|
| John the Ripper (CLI) | Crack password hashes, including from PDF files |
| Johnny | GUI front-end for John the Ripper |
| onlinehashcrack.com (PDF Hash Extractor) | Extract a crackable hash from a locked PDF |
| NetworkWalks Hash Calculator | Extract a crackable hash from a locked PDF, entirely in-browser |
| NetworkWalks Password Cracker | Dictionary attack against a PDF hash, in-browser |

---

# 🔑 Module 1: Password Cracking with JTR (John & Johnny)

Target file: `My Locked PDF1.pdf`

## Step 1: Extract the hash

Uploaded the locked PDF to the online PDF Hash Extractor, which uses `pdf2john` to pull out a crackable hash in the format `$pdf$...`. Saved this hash into `hash1.txt`.

## Step 2: Crack it with Johnny (GUI)

- Opened **Johnny**, set the John the Ripper executable path (`john.exe`)
- Opened `hash1.txt` as the password file
- Clicked **Start new attack**


**Result:** password cracked → `good-luck`

## Step 3: Open the PDF

Entered `good-luck` to unlock the PDF and captured the flag.

![Johnny cracking the hash](screenshots/johnny%20screen.png)

**How I read it:** Johnny is just a GUI wrapper around John the Ripper. Underneath, it runs the same dictionary attack, trying each word from John's default wordlist (`password.lst`) against the hash until one matches.

---

# 🔑 Module 2: Password Cracking with NetworkWalks Tools

Target file: `My Locked PDF1.pdf (NW tools)`

## Step 1: Extract the hash with the Hash Calculator

Uploaded the PDF to the **NetworkWalks Hash Calculator**. The tool parses the PDF entirely in the browser (nothing is uploaded to a server) and returns the crackable hash.


## Step 2: Crack it with the Password Cracker

Pasted the hash into the **NetworkWalks Password Cracker** and ran the dictionary attack.

![NW Password Cracker](screenshots/NW%20PASS%20CRACK.png)

**Result:** password cracked: `password1`

**How I read it:** This tool works exactly like John the Ripper conceptually, hash every word in a wordlist and compare it to the target hash, just implemented as a web page instead of a command-line tool.

---

#  Bonus: Cracking a PDF directly with John (CLI)

As extra practice, I tested the same locked PDF using John the Ripper directly from the command line, on both Kali Linux and Windows, without using Johnny.

## On Kali Linux

```bash
john hash.txt
john --show hash.txt
```


**Result:** `?:good-luck` → `1 password hash cracked, 0 left`

**How I read it:** Running `john hash.txt` a second time said "No password hashes left to crack", because John keeps a local cache (`john.pot`) of hashes it already cracked; `--show` reads that cache instead of redoing the work.


## On Windows (same file, same hash)

```cmd
john.exe hash.txt
john.exe --show hash.txt
```

**Result:** Same outcome, `1 password hash cracked, 0 left`, confirming John the Ripper behaves the same way across platforms.

---

# 💡 Key Learnings

- **Hashing vs. encryption:** A PDF password isn't stored as plain text. Cracking means extracting the hash the PDF's encryption produces, then testing candidate passwords against it, not "decrypting" the password directly.
- **Weak passwords fall instantly.** Both `password1` and `good-luck` were cracked in seconds with a small, common wordlist.
- **GUI, CLI, and web tools reach the same result.** Johnny, raw John, and NetworkWalks' web tool all do the same underlying work; the interface just changes how you interact with it.
- **Cross-platform consistency:** the same hash, cracked the same way, gave the same password on both Kali and Windows.
- **A dictionary attack is only as good as its wordlist.** The tool can't guess a password that isn't in the list it's trying. A genuinely random, complex password defeats this attack entirely, no wordlist will contain it.

---

## 👤 Author

**Furat Omer**
Cybersecurity Intern @ Network Walks (Batch B084)

[![GitHub](https://img.shields.io/badge/GitHub-FuratOmer-181717?logo=github&logoColor=white)](https://github.com/FuratOmer)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Furat%20Omer-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/furat-omer-a195572b4)

---

*This repository reflects my ongoing learning journey in cybersecurity.*
