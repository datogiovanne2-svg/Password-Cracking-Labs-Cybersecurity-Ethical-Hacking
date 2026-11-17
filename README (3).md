# Password Cracking Labs — Cybersecurity & Ethical Hacking

Hands-on write-up of two password cracking exercises completed as part of a
Cybersecurity & Ethical Hacking training program (Week 3, Project Modules 1 & 2).
The goal of both labs was to recover the password of a locked PDF file,
using two different tool sets, to understand how password hashing and
cracking work in practice.

> ⚠️ **Disclaimer:** This work was carried out on a file provided for training
> purposes in a controlled lab environment. Password cracking should only
> ever be performed on systems or files you own, or have explicit written
> permission to test. The password and flag values used in this repo have
> been omitted — check your own lab output if you need them for submission.

---

## Overview

| Lab | Tools | Method |
|---|---|---|
| Module 1 | John the Ripper (JTR) + Johnny GUI | Dictionary attack via GUI |
| Module 2 | Networkwalks Hash Calculator + Password Cracker | Browser-based dictionary attack |

Both labs targeted the same encrypted file (`My Locked PDF1.pdf`) and used the
same underlying concept: extract the PDF's password hash (`$pdf$...` format,
compatible with `pdf2john` / hashcat), then run that hash through a
dictionary-based cracking tool until a match is found.

---

## Lab 1: Password Cracking with John the Ripper (JTR)

**Tools:** John the Ripper, Johnny (GUI front-end for JTR)

**Steps:**
1. **Downloaded John the Ripper** from the official Openwall site
   (`openwall.com/john/`), choosing the Windows jumbo build.
2. **Downloaded Johnny**, the GUI front-end for JTR, from the Openwall wiki
   (`openwall.info/wiki/john/johnny`).
3. **Installed Johnny** by running the installer executable, then launched it
   from the desktop shortcut.
4. **Pointed Johnny at John's executable:**
   - Opened Johnny → clicked **Settings**.
   - Clicked **Browse** next to "John the Ripper executable."
   - Navigated into the extracted JTR folder → `run` subfolder → selected
     `john.exe`.
   - Johnny confirmed it detected a valid JTR build and version.
5. **Extracted the PDF's hash:**
   - Downloaded the locked file (`My Locked PDF1.pdf`) to the PC.
   - Opened an online `pdf2john` / PDF hash extractor tool in the browser.
   - Uploaded the PDF and clicked **Upload** to generate the hash.
   - Copied the output hash, which starts with `$pdf$...`.
6. **Saved the hash to a file:**
   - Opened Notepad, pasted the hash in.
   - Checked for and removed any stray characters (e.g. a leading `b'`) that
     some extractors add, since JTR expects a clean `$pdf$...` string.
   - Saved the file as `hash1.txt`.
7. **Loaded the hash into Johnny:**
   - Clicked **Open password file** → selected **PASSWD format**.
   - Browsed to `hash1.txt` and clicked **Open**.
   - Johnny listed one entry with the hash and format shown as `PDF`.
8. **Ran the attack:**
   - Clicked **Start new attack**.
   - JTR worked through its wordlist in the background; progress and cracked
     count updated at the bottom of the window.
9. **Recovered the password:** once finished, the cracked password appeared
   directly in the **Password** column next to the hash entry.
10. **Verified it:** opened the encrypted PDF, entered the recovered password
    at the prompt, and confirmed the file unlocked successfully.

**Key takeaway:** JTR is the industry-standard CLI/GUI cracking tool, flexible
enough to handle many hash and file types (PDF, ZIP, Office documents, Unix/Windows
credentials, and more).

---

## Lab 2: Password Cracking with Networkwalks Tools

**Tools:** Networkwalks Hash Calculator, Networkwalks Password Cracker (browser-based)

**Steps:**
1. **Downloaded the locked PDF** (`My Locked PDF1.pdf`) from the lab page.
2. **Opened the Networkwalks Hash Calculator** (`networkwalks.com/hash-calculator/`)
   in a browser.
3. **Switched to the PDF tab** of the calculator (it also supports hashing
   plain text and files, but the PDF tab is what extracts a crackable hash
   from an encrypted document).
4. **Uploaded the locked PDF** by clicking to choose the file or dragging it
   into the drop zone. The page noted that nothing is uploaded to a server —
   the file is parsed locally in-browser.
5. **Read the extracted hash:** the tool confirmed the PDF was encrypted and
   displayed a crackable hash in `pdf2john` / hashcat-compatible format,
   starting with `$pdf$...`, along with metadata (Revision, Version, Key length).
6. **Copied the full hash** using the **Copy** button, making sure to capture
   the entire string starting from `$pdf$`.
7. **Opened the Networkwalks Password Cracker**
   (`networkwalks.com/password-cracker/`) in a new tab.
8. **Pasted the hash** into the **PDF HASH** input field.
9. **Selected the wordlist:** left the default **Built-in list (100 passwords)**
   active (an option to upload a custom `.txt` wordlist is also available).
10. **Clicked Start Cracking.** The tool streamed its attempts live — showing
    each password it tried and whether it matched — along with a progress bar
    and tries-per-second counter.
11. **Got the match:** once a candidate password matched the hash, the tool
    displayed **"Password Cracked Successfully"** along with the recovered
    password and a **Copy password** button.
12. **Verified it:** opened the locked PDF, entered the recovered password at
    the Document Open Password prompt, and confirmed the file unlocked.

**Key takeaway:** these lightweight browser tools are a good way to visualize
*how* a dictionary attack works (trying word after word against a hash) without
installing anything.

---

## What I learned

- A locked PDF doesn't store its password in plain text — it stores a **hash**,
  a one-way scrambled representation of it.
- Hashing is irreversible, but that doesn't make a weak password safe: if the
  original password is short, common, or in a wordlist, a dictionary attack
  will find it in seconds.
- Both a professional CLI tool (JTR) and a simple browser tool use the exact
  same underlying technique — this is the core idea behind every dictionary
  attack, no matter how sophisticated the tool.
- **Defense takeaway:** use long, unique passphrases and a password manager.
  A simple 8-character lowercase password can fall in minutes; a long, mixed
  12+ character password can take years.

---

## References

- [John the Ripper](https://www.openwall.com/john/)
- [Johnny GUI](https://openwall.info/wiki/john/johnny)
- [Networkwalks Hash Calculator](https://networkwalks.com/hash-calculator/)
- [Networkwalks Password Cracker](https://networkwalks.com/password-cracker/)
- Training material: Networkwalks Academy — Cybersecurity & Ethical Hacking
