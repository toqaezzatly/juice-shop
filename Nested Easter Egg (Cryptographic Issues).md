Here's the refined write-up for the **easter.gg** decryption challenge:

---

# Juice Shop Lab - Decrypting `easter.gg` File

This write-up details the steps taken to locate and decrypt an encrypted file named `easter.gg` in a Juice Shop lab. The challenge requires finding hidden files, bypassing file extension restrictions using null-byte injection, and decrypting encoded content using Base64 and ROT13.

---

## Overview

### Challenge Process:
1. **Locating the `easter.gg` file** using brute force or URL manipulation.
2. **Downloading the file** by bypassing unwanted extensions using null-byte injection.
3. **Decrypting the file's content**, which is first encoded in Base64 and then encrypted with the ROT13 cipher.

### Tools Used:
- **CyberChef** and **Command-line**: For Base64 decoding and ROT13 decryption.
- **Burp Suite** and **gobuster**: For brute-forcing and intercepting requests.

---

## Steps to Solve the Challenge

### 1. Discovering the `easter.gg` File

Using a combination of manual exploration and brute-forcing tools, the file `easter.gg` was discovered at `http://localhost:3000/ftp/eastere.gg`. When attempting to download it, the server appends `.md` to the file, requiring a null-byte injection to retrieve the original file.

### 2. Downloading the `easter.gg` File

By injecting a null-byte (`%00`) into the URL, we bypass the `.md` extension:
- **Modified URL**: `http://localhost:3000/ftp/eastere.gg%2500.md`

### 3. Decrypting the File

After downloading the file, the following steps were used to decrypt its contents:

- **Base64 Decoding**: The content was decoded from Base64 using CyberChef or command-line utilities.
- **ROT13 Decryption**: The decoded result was then decrypted using the ROT13 cipher to reveal the final message.

---

## Impact

- **Sensitive File Exposure**: The lab demonstrates how misconfigured file access and weak encoding practices can expose critical information.
- **Exploitation Techniques**: Null-byte injection and weak encryption (Base64 + ROT13) can be easily reversed using standard tools, allowing attackers to bypass security measures.

---

## Vulnerability Assessment

- **Severity Score**: Medium-High (6.5/10)
- **Impact Score**: High, as it leads to unauthorized access and decryption of sensitive files.