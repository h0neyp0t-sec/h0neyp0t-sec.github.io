---
title: TryHackMe Advent of Cyber 2025 Day 9
summary: Walkthrough of the TryHackMe Advent of Cyber 2025 Day 9 - Passwords - A Cracking Christmas
date: 2025-12-12
categories:
  - walkthrough
tags:
  - TryHackMe
  - ctf
  - AdventOfCyber2025
cover:
  image: /2025/aoc_2025_day_9/Advent_of_Cyber_2025_Day_9_cover.png
  hidden: false
  responsiveImages: true
draft: false
---
# Introduction
## The Story

>With time between Easter and Christmas being destabilised, the once-quiet systems of _The Best Festival Company_ began showing traces of encrypted data buried deep within their servers. Sir Carrotbane, stumbled upon a series of locked PDF and ZIP files labelled _“North Pole Asset List.”_ Rumours spread that these could contain fragments of **Santa’s master gift registry**, critical information that could help Malhare control the festive balance between both worlds.

>Sir Carrotbane sets out to crack the encryption, learning how weak passwords can expose even the most guarded secrets. Can the Elves adapt fast and prevent their secrets from being discovered?

## Learning Objectives

- How password-based encryption protects files such as PDFs and ZIP archives.
- Why weak passwords make encrypted files vulnerable.
- How attackers use dictionary and brute-force attacks to recover passwords.
- A hands-on exercise: cracking the password of an encrypted file to reveal its contents.
- The importance of using strong, complex passwords to defend against these attacks.

>A few simple points to remember:
> - The **strength of protection** depends almost entirely on the password. Short or common passwords can be guessed; long, random passwords are far harder to break.
> - Different file formats use different algorithms and key derivation methods. For example, PDF encryption and ZIP encryption differ in details (how the key is derived, salt use, number of hash iterations). That affects how easy or hard cracking is.
> - Many consumer tools still support legacy or weak modes (particularly older ZIP encryption). That makes some encrypted archives much easier to attack than modern, well-implemented schemes.
> - Encryption protects data confidentiality only. It does not prevent someone with access to the encrypted file from trying to guess the password offline.

>To make it simple, encryption makes the contents unreadable unless the correct password is known. If the password is weak, an attacker can simply try likely passwords until one works.

# Attacks Against Encrypted Files

## How Attackers Recover Weak Passwords

>Attackers don't usually try to "break" the encryption itself because that would take far too long with modern cryptography. Instead, they focus on guessing the password that protects the file. The two most common ways of doing this are dictionary attacks and brute-force (or mask) attacks.

### Dictionary Attacks

>In a dictionary attack, the attacker uses a predefined list of potential passwords, known as a wordlist, and tests each one until the correct password is found. These wordlists often contain leaked passwords from previous breaches, common substitutions like **password123**, predictable combinations of names and dates, and other patterns that people frequently use. Because many users choose weak or common passwords, dictionary attacks are usually fast and highly effective.

Attackers can try to craft a custom dictionary based on data gathered on the target (name, city, company, birthdate etc. )

### Mask Attack 

>Brute-force and mask attacks go one step further. A brute-force attack systematically tries every possible combination of characters until it finds the right one. While this guarantees success eventually, the time it takes grows exponentially with the length and complexity of the password.

>Mask attacks aim to reduce that time by limiting guesses to a specific format. For example, trying all combinations of three lowercase letters followed by two digits.

>By narrowing the search space, mask attacks strike a balance between speed and thoroughness, especially when the attacker has some idea of how the password might be structured.

>Practical tips attackers use (and defenders should know about):
>- Start with a wordlist (fast wins). Common lists: `rockyou.txt`, `common-passwords.txt`.
>- If the wordlist fails, move to targeted wordlists (company names, project names, or data from the target).
>- If that fails, try mask or incremental attacks on short passwords (e.g. `?l?l?l?d?d` = three lowercase letters + two digits, which is used as a password mask format by password cracking tools).
>- Use GPU-accelerated cracking when possible; it dramatically speeds up attacks for some algorithms.
>- Keep an eye on resource use: cracking is CPU/GPU intensive. That behaviour can be detected on a monitored endpoint.


## Exercice

>You will find the files for this section in the `Desktop` directory of the machine. Switch to it by running `cd Desktop` in your terminal.


**1. Confirm the File Type**

>Use the `file` command or open the file with a hex viewer. This helps pick the right tool

```shell-session
ubuntu@tryhackme:~/Desktop$ file flag.pdf      ubuntu@tryhackme:~/Desktop$ file flag.zip
```

>If it's a PDF, proceed with PDF tools. If it's a ZIP, proceed with ZIP tools.

You may wonder why they said "open the file with a hex viewer" and how to do it. This is because each file type has a specific structure and the first hexadecimal values define the file signature (aka the "magic bytes"), you can find a list on [Wikipedia](https://en.wikipedia.org/wiki/List_of_file_signatures). 

To view the hexadecimal values of a file, you can use the `xxd` command (Linux) : 

```shell
┌──(attacker㉿kali)-[~]
└─$ xxd flag.pdf                               
00000000: 2550 4446 2d31 2e37 0d0a 25b5 b5b5 b50d  %PDF-1.7..%.....
00000010: 0a31 2030 206f 626a 0d0a 3c3c 2f54 7970  .1 0 obj..<</Typ
00000020: 652f 4361 7461 6c6f 672f 5061 6765 7320  e/Catalog/Pages 
00000030: 3220 3020 522f 4c61 6e67 285d 88be 6c25  2 0 R/Lang(]..l%
00000040: a162 ed97 ad5d e33d 5bae 2120 a186 3434  .b...].=[.! ..44
00000050: 4769 6132 4d1b 7d08 44b0 5d29 202f 5374  Gia2M.}.D.]) /St
00000060: 7275 6374 5472 6565 526f 6f74 2031 3520  ructTreeRoot 15 
00000070: 3020 522f 4d61 726b 496e 666f 3c3c 2f4d  0 R/MarkInfo<</M
00000080: 6172 6b65 6420 7472 7565 3e3e 2f4d 6574  arked true>>/Met
00000090: 6164 6174 6120 3237 2030 2052 2f56 6965  adata 27 0 R/Vie
000000a0: 7765 7250 7265 6665 7265 6e63 6573 2032  werPreferences 2
000000b0: 3820 3020 523e 3e0d 0a65 6e64 6f62 6a0d  8 0 R>>..endobj.

```

As you can see, the file begins with `25 50 44 46 2D` in hexadecimal or `%PDF-` which is the standard file signature of the PDF format. The system will know it's a PDF file even if you change the extension.

Let's see what happens if I change the file name :

```shell-session
┌──(attacker㉿kali)-[~]
└─$ mv flag.pdf flag.txt
```

Now if I check its file type it is still the same. 

```shell-sesion
┌──(attacker㉿kali)-[~]
└─$ file flag.txt                   
flag.txt: PDF document, version 1.7, 1 page(s)
```


**2. Tools to Use (pick one based on file type)**

>- PDF: `pdfcrack`, `john` (via `pdf2john`)
>- ZIP: `fcrackzip`, `john` (via `zip2john`)
>- General: `john` (very flexible) and `hashcat` (GPU acceleration, more advanced)

**3. Try a Dictionary Attack First (fast, often successful)**

>Example: PDF with `pdfcrack` and `rockyou.txt`:

```shell-session
ubuntu@tryhackme:~/Desktop$ pdfcrack -f flag.pdf -w /usr/share/wordlists/rockyou.txt 
PDF version 1.7
Security Handler: Standard
V: 2
R: 3
P: -1060
Length: 128
Encrypted Metadata: True
FileID: 3792b9a3671ef54bbfef57c6fe61ce5d
U: c46529c06b0ee2bab7338e9448d37c3200000000000000000000000000000000
O: 95d0ad7c11b1e7b3804b18a082dda96b4670584d0044ded849950243a8a367ff

Average Speed: 29538.5 w/s. Current Word: 's8t8a8r8'  
Average Speed: 31068.0 w/s. Current Word: 'toby344'  
Average Speed: 30443.7 w/s. Current Word: 'erikowa'  
Average Speed: 29519.3 w/s. Current Word: '0848845800'  
Average Speed: 29676.4 w/s. Current Word: 'u3904041'  
Average Speed: 29339.8 w/s. Current Word: 'spider0187'  
Average Speed: 30156.8 w/s. Current Word: 'roermond7'  
Average Speed: 30364.8 w/s. Current Word: 'papaopass2'  
found user-password: 'XXXXXXXXXX'  
```

>Example: using `john` 
>- Create a hash that John can understand: `zip2john flag.zip > ziphash.txt`

```shell-session
ubuntu@tryhackme:~/Desktop$ zip2john flag.zip > ziphash.txt     
```

- John will report the recovered password if it finds one.

```shell-session
ubuntu@tryhackme:~/Desktop$ john --wordlist=/usr/share/wordlists/rockyou.txt ziphash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 128/128 AVX 4x])
Cost 1 (HMAC size) is 29 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status

XXXXXXXXXXX      (flag.zip/flag.txt)     
1g 0:00:02:12 DONE (2025-10-03 08:26) 0.007552g/s 20696p/s 20696c/s 20696C/s winternights..wine23

Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

## Detection of Indicators and Telemetry

>Offline cracking does not hit login services, so lockouts and failed logon dashboards stay quiet. We can detect the work where it runs, on endpoints and jump boxes. The important signals to monitor include:

>**Process creation:** Password cracking has a small set of well-known binaries and command patterns that we can look out for. A mix of process events, file activity, GPU signals, and network touches tied to tooling and wordlists. Our goal is to make the activity obvious without drowning in noise.
>- Binaries and aliases: `john`, `hashcat`, `fcrackzip`, `pdfcrack`, `zip2john`, `pdf2john.pl`, `7z`, `qpdf`, `unzip`, `7za`, `perl` invoking `pdf2john.pl`.
>- Command‑line traits: `--wordlist`, `-w`, `--rules`, `--mask`, `-a 3`, `-m` in Hashcat, references to `rockyou.txt`, `SecLists`, `zip2john`, `pdf2john`.
>- Potfiles and state: `~/.john/john.pot`, `.hashcat/hashcat.potfile`, `john.rec`.

>It's worth noting that on Windows systems, Sysmon Event ID 1 captures process creation with full command line properties, while on Linux, `auditd`, `execve`, or EDR sensors capture binaries and arguments.

### GPU and Resource Artefacts

>GPU cracking is loud. Sudden high utilisation on hosts can be picked up and would need to be investigated.
>- `nvidia-smi` shows long‑running processes named `hashcat` or `john`.
>- High, steady GPU utilisation and power draw while the fan curve spikes.
>- Libraries loaded: `nvcuda.dll`, `OpenCL.dll`, `libcuda.so`, `amdocl64.dll`.

### Network Hints, Light but Useful

>Offline cracking does not need the network once wordlists are present. Yet most operators fetch lists and tools first.
>- Downloads of large text files named `rockyou.txt`, or Git clones of popular wordlist repos.
>- Package installs, for example `apt install john hashcat`, detected by EDR package telemetry.
>- Tool updates and driver fetches for GPU runtimes.

### Unusual File Reads

>Repeated reads of files such as wordlists or encrypted files would need analysis.

### Detection

>Below are some examples of detection rules and hunting queries we can put to use across various environments.

>_Sysmon_:

```EventID=1
(ProcessName="C:\Program Files\john\john.exe" OR
 ProcessName="C:\Tools\hashcat\hashcat.exe" OR
 CommandLine="*pdf2john.pl*" OR
 CommandLine="*zip2john*")
```

>_Linux audit rules, temporary for an investigation_:
```bash
auditctl -w /usr/share/wordlists/rockyou.txt -p r -k wordlists_read
auditctl -a always,exit -F arch=b64 -S execve -F exe=/usr/bin/john -k crack_exec
auditctl -a always,exit -F arch=b64 -S execve -F exe=/usr/bin/hashcat -k crack_exec
```

>_Sigma style rule, Windows process create for cracking tools_:

```yaml
title: Password Cracking Tools Execution
id: 9f2f4d3e-4c16-4b0a-bb3a-7b1c6c001234
status: experimental
logsource:
  product: windows
  category: process_creation
detection:
  selection_name:
    Image|endswith:
      - '\john.exe'
      - '\hashcat.exe'
      - '\fcrackzip.exe'
      - '\pdfcrack.exe'
      - '\7z.exe'
      - '\qpdf.exe'
  selection_cmd:
    CommandLine|contains:
      - '--wordlist'
      - 'rockyou.txt'
      - 'zip2john'
      - 'pdf2john'
      - '--mask'
      - ' -a 3'
  condition: selection_name or selection_cmd
level: medium
```

### Response Playbook

>As security analysts in Wareville, it is important to have a playbook to follow when such incidents occur. The immediate actions to take are:
>1. Isolate the host if malicious activity is detected. If it is a lab, tag and suppress.
>2. Capture triage artefacts such as process list, process memory dump, `nvidia-smi` sample output, open files, and the encrypted file.
>3. Preserve the working directory, wordlists, hash files, and shell history.
>4. Review which files were decrypted. Search for follow‑on access, lateral movement or exfiltration.
>5. Identify the origin and intent of the activity. Was this authorised? If not, escalate to the IR team.
>6. Remediate the activity, rotate affected keys and passwords, and enforce MFA for accounts.
>7. Close with education and correct placement of tools into approved sandboxes.

# Questions
## What is the flag inside the encrypted PDF?

`THM{Cr4ck1ng_PDFs_1s_****}`
## What is the flag inside the encrypted zip file?

`THM{Cr4ck1n6_z1p$_1s_*******}`

If you are connecting via SSH, use sftp to get both files on your local machine to be able to open them.

# Conclusion

Password cracking is a must-have skill as a pentester but also as a defender. I love how they taught us to both attack and defend. 

Never stop learning.

[h0neyp0t](https://www.instagram.com/h0neyp0t.sec/)
