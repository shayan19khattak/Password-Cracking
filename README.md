# Password Cracking using JTR and NW tools

A comprehensive cybersecurity and ethical hacking lab project documenting end-to-end PDF password recovery, cryptographic hash extraction, and offline dictionary attacks[cite: 2, 18]. This repository covers two complete methodologies: a desktop-based auditing workflow using **John the Ripper (JTR)** with the **Johnny GUI**[cite: 2, 3], and a browser-based local parsing and dictionary attack workflow using **Networkwalks Security Tools**[cite: 18, 19].


---

## Lab Objectives & Background

Password-protected documents (PDF, ZIP, Microsoft Office) rely on cryptographic hashes derived from user-supplied passwords to restrict unauthorized viewing or editing[cite: 2, 18]. Security professionals perform password auditing to evaluate credential strength and demonstrate how weak, short, or common passwords expose sensitive files to rapid offline recovery[cite: 2, 18].

The objectives of this lab are:
- Understand the architectural difference between reversible encryption and one-way cryptographic hashing[cite: 15].
- Extract `pdf2john` / `hashcat`-compatible `$pdf$` cryptographic hashes from an encrypted PDF file using both online extractors and client-side parsers[cite: 7, 19, 20].
- Configure **John the Ripper Jumbo** and link its core binary (`john.exe`) to the **Johnny** graphical frontend on Windows[cite: 3, 4, 5].
- Execute offline dictionary attacks to recover the plaintext Document Open Password (`password1`) and verify full access to the secured target file[cite: 12, 13, 14, 21, 22, 23].

---

## Core Cryptographic Concepts

### Encryption vs. Hashing
| Feature | Encryption | Hashing |
| :--- | :--- | :--- |
| **Directionality** | Two-way function (reversible)[cite: 15] | One-way function (irreversible)[cite: 15] |
| **Mechanism** | Transforms plaintext into ciphertext using an encryption algorithm and key[cite: 15] | Scrambles plaintext through a mathematical hash function to produce a fixed-length message digest[cite: 15] |
| **Recovery Method** | Decrypted directly using the authorized key[cite: 15] | Cannot be decrypted; recovered by hashing candidate words from a wordlist and comparing digests until a match is found[cite: 15, 18, 21] |
| **Primary Purpose** | Confidentiality and protecting sensitive data in transit or at rest[cite: 15] | Data integrity validation and secure credential verification[cite: 15] |

### How PDF Password Auditing Works
1. **Hash Extraction:** Instead of repeatedly opening a PDF reader and typing passwords manually, an extraction utility (`pdf2john`) parses the PDF header/encryption dictionary to isolate the password hash parameters[cite: 7, 18, 20].
2. **Offline Attack Execution:** The extracted hash is fed into a cracking engine alongside a wordlist[cite: 11, 21]. The engine hashes each candidate password using the exact PDF encryption parameters and compares the output against the target hash[cite: 21].
3. **Verification:** Once a digest collision/match occurs, the plaintext password is revealed and used to unlock the document[cite: 13, 14, 22, 23].

---

## Environment & Tools Used

- **Operating System:** Windows 10/11 (x64) with Adobe Acrobat Reader DC (32-bit) for verification[cite: 4, 14] (also reproducible on Kali Linux where JTR is pre-installed)[cite: 2, 18].
- **John the Ripper (`1.9.0-jumbo-1` 64-bit Windows Binaries):** Open-source password security auditing and recovery suite supporting hundreds of hash and cipher types, including PDF, ZIP, RAR, Office documents, and Unix/Windows hashes.
- **Johnny GUI (`v2.2` Windows Binary):** Cross-platform graphical frontend for John the Ripper that provides point-and-click session management, progress tracking, and hash loading[cite: 2, 3, 4].
- **OnlineHashCrack PDF Hash Extractor:** Web utility wrapping `pdf2john` to convert locked `.pdf` files into raw `$pdf$` hash strings.
- **Networkwalks Hash Calculator:** Browser-based cryptographic utility supporting MD5, SHA-1, SHA-256, SHA-384, SHA-512, and local client-side PDF hash extraction without uploading files to a remote server[cite: 18, 19, 20].
- **Networkwalks Password Cracker:** Browser-based dictionary attack lab tool supporting a built-in 100-password wordlist as well as custom `.txt` wordlist uploads.

---

## Target File & Hash Anatomy

### Target Document Details
- **Filename:** `My Locked PDF1.pdf` (internal document title: `networkwalks_flag1.pdf`)[cite: 2, 14]
- **File Size:** `65.2 KB` (`66 KB` on disk)[cite: 14, 20]
- **PDF Encryption Specification:** Version `V4`, Revision `R4`, `128-bit` Key Length
- **Recovered Credential:** `password1`[cite: 13, 22]

### Extracted Hash String
```text
$pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*777fd021a7f3c5ae598c0c6495c7f76e00000000000000000000000000000000*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1
