# Juice Shop Lab - Decrypting `easter.gg` File


This file walks through the process of finding and decrypting an encrypted file named `easter.gg` in a Juice Shop lab. The lab involves discovering hidden files, downloading them using URL manipulation, and decrypting their contents.

## Overview
The challenge involves:
- Discovering the location of the `easter.gg` file.
- Downloading the file using a null-byte injection trick.
- Decrypting the file, which was encoded using the ROT13 cipher.

### Tools and Techniques Used:
- Directory Brute Forcing
- URL Manipulation (Null-byte injection)
- ROT13 Decryption

---

## 1. Discovering the `easter.gg` File

### Steps to Find the File:
1. **Manual Exploration**:
   Initially, to find hidden files, navigate to common paths in the application such as:
   - `/ftp/` or `/files/` which are often used to store downloadable files.
   - By trying to access `http://localhost:3000/ftp/`, I checked if directory browsing was enabled. This could potentially reveal a list of files, but it wasn't in this case.
   - 
   ![image](https://github.com/user-attachments/assets/5adadca5-60aa-4376-8dca-2152c2b25122)



2. **Using Brute Force**:
   If manual exploration doesn’t work, tools like `dirb`, `gobuster`, or `ffuf` can help brute force directories and file names. For instance:
   ```bash
   gobuster dir -u http://localhost:3000/ -w /path/to/wordlist.txt
   ```
   These tools generate possible file and directory names based on a wordlist and test them on the server.

3. **Finding the File**:
   In this lab, the file was found at `http://localhost:3000/ftp/eastere.gg`. This path might have been uncovered by checking common folders or receiving clues in the Juice Shop interface.

---

## 2. Downloading the `eastere.gg` File

### File Download Method:
In the Juice Shop app, the file `eastere.gg` was located in the `/ftp/` directory, but when accessed directly, the app would append `.md` to render it as a Markdown file. 

Here’s where **null-byte injection** comes in.

### What is Null-byte Injection?
Null-byte (`%00`) is used to terminate strings in many programming languages, especially C-based ones. Some servers and applications interpret `%00` as the end of the file name, allowing us to trick the system into ignoring appended extensions like `.md`.

### Steps to Download the File:
To bypass the automatic `.md` extension, append `%2500.md` to the file name in the URL:
- **Original URL**: `http://localhost:3000/ftp/eastere.gg`
- **Modified URL**: `http://localhost:3000/ftp/eastere.gg%2500.md`

Here, `%2500` is the URL-encoded version of the null byte (`%00`). This tricks the server into serving the raw `eastere.gg` file instead of treating it as a Markdown file.

---

## 3. Decrypting the File

### Decryption Process:
Once the `eastere.gg` file is downloaded, its contents need to be decrypted. In this case, the encryption used is **ROT13**.

### What is ROT13?
ROT13 is a simple Caesar cipher where each letter is shifted by 13 places in the alphabet. For example:
- 'A' becomes 'N'
- 'B' becomes 'O'
- 'C' becomes 'P', and so on.

It is often used for obfuscation, especially in CTFs and other security challenges, because it's easy to encode and decode.

### Steps to Decrypt Using ROT13:
You can use several methods to decode ROT13:
- Online ROT13 decoder tools.
- Linux terminal command:
  ```bash
  echo "EncryptedText" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
  ```
  This command translates each letter into its corresponding ROT13-encoded counterpart.

For example:
```bash
echo "Guvf vf zl frperg zrffntr!" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
This outputs:
```
This is my secret message!
```

### Example Output:
After decoding the `eastere.gg` file, you should see the decrypted message.

---

## Conclusion

This Juice Shop lab walks through several important concepts used in web application security:
- **File Discovery**: Using directory brute-forcing or manual exploration to find hidden or obscure files.
- **Null-byte Injection**: A technique for bypassing file extension restrictions by injecting a null byte (`%00`) to terminate the string prematurely.
- **ROT13 Decryption**: A basic cipher often used in CTFs to obfuscate text.

By understanding these techniques, you can better analyze and exploit vulnerabilities in web applications.

---

## Additional Notes
- Tools like `Burp Suite` can assist in inspecting requests and trying different payloads for directory brute-forcing or file access.
- Remember to always practice ethical hacking and obtain permission before testing web applications in real-world scenarios.
