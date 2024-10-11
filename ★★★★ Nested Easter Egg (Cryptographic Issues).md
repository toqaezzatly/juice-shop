# Juice Shop Lab - Decrypting `easter.gg` File

This document walks through the process of locating and decrypting an encrypted file named `easter.gg` in a Juice Shop lab. The lab requires discovering hidden files, downloading them using URL manipulation, and decrypting their contents.

## Overview
The challenge involves:
- Finding the `easter.gg` file.
- Downloading the file using a null-byte injection trick.
- Decrypting the file, which was first Base64 encoded, followed by a ROT13 cipher.

### Tools and Techniques Used:
- Directory Brute Forcing
- URL Manipulation (Null-byte injection)
- Base64 Decoding
- ROT13 Decryption

---

## 1. Discovering the `easter.gg` File

### Steps to Find the File:
1. **Manual Exploration**:
   Initially, to locate hidden files, navigate to common paths in the application such as:
   - `/ftp/`, `/files/`, or `/downloads/`—directories often used for downloadable content.
   
   You can check if directory listing is enabled by visiting URLs like:
   - `http://localhost:3000/ftp/`
   - `http://localhost:3000/files/`
   
   If the server allows directory browsing, it might show a list of available files. In this case, directory listing was not enabled, so further exploration was needed.

2. **Using Brute Force**:
   To automate the search for hidden files, tools like `gobuster`, `dirb`, or `ffuf` can brute-force directories and file names. Here is an example of using `gobuster` to find the file:
   ```bash
   gobuster dir -u http://localhost:3000/ -w /path/to/wordlist.txt
   ```
   These tools use wordlists to guess common directories and file names.

3. **Finding the File**:
   In this lab, the file was eventually found at `http://localhost:3000/ftp/eastere.gg`. This might have been uncovered by brute-forcing or through hints in the Juice Shop interface.

---

## 2. Downloading the `eastere.gg` File

### File Download Method:
Once the file `eastere.gg` is located, you may notice that attempting to download it directly results in the server appending `.md` (Markdown format) to the file. The goal here is to bypass this behavior and download the raw file.

### What is Null-byte Injection?
Null-byte (`%00`) injection is used to terminate strings prematurely in certain programming languages, tricking the server into ignoring any added file extensions like `.md`. By using a null byte, we can request the original file format.

### Steps to Download the File:
To bypass the `.md` extension, append `%2500.md` to the file name in the URL:
- **Original URL**: `http://localhost:3000/ftp/eastere.gg`
- **Modified URL**: `http://localhost:3000/ftp/eastere.gg%2500.md`

Here, `%2500` is the URL-encoded version of the null byte (`%00`). This tells the server to ignore the `.md` extension, allowing you to download the `eastere.gg` file.

---

## 3. Decrypting the File

### Decryption Process:
After successfully downloading the file, you need to decrypt its contents. The file is first Base64 encoded and then encrypted using the ROT13 cipher.

### Step 1: Base64 Decoding
Base64 is an encoding scheme used to represent binary data in an ASCII string format. You can decode the Base64-encoded text using:
- **Linux Command**:
   ```bash
   echo "L2d1ci9xcmlmL25lci9mYi9zaGFhbC9ndXJsL3V2cS9uYS9ybmZncmUvcnR0L2p2Z3V2YS9ndXIvcm5mZ3JlL3J0dA==" | base64 --decode
   ```

### Step 2: ROT13 Decryption
Once you’ve decoded the Base64 string, you can apply ROT13 to the result. ROT13 is a Caesar cipher where each letter is shifted by 13 positions in the alphabet.

- **Linux Command**:
   ```bash
   echo "decoded_string_here" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
   ```

Alternatively, you can use online tools like [CyberChef](https://gchq.github.io/CyberChef/) to perform the Base64 decoding and ROT13 decryption.

---

## Conclusion

This Juice Shop lab covers several important concepts used in web application security:
- **File Discovery**: Using brute-forcing techniques or manual exploration to find hidden or obscure files.
- **Null-byte Injection**: Exploiting file path vulnerabilities by injecting a null byte (`%00`) to bypass unwanted extensions.
- **Base64 Decoding and ROT13 Decryption**: Techniques often used in CTFs to obfuscate and decrypt text.

By understanding these techniques, you can enhance your skills in finding and exploiting vulnerabilities in web applications.

---

## Additional Notes
- Tools like `Burp Suite` are useful for inspecting requests, testing payloads, and brute-forcing directory structures.
- Always remember to practice ethical hacking and obtain permission before testing web applications in real-world environments.

