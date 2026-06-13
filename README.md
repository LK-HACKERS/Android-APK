# 🛡️ APK Obfuscator & Encryptor for Metasploit Payloads

A powerful **Bash framework** to automatically generate, obfuscate, repack, sign, and align Android payloads (MSFvenom) to bypass **Google Play Protect** and common antivirus signature detection.

> ✅ Works on **Termux**, **Kali Linux**, **Ubuntu**, and **macOS**

---

## 🔍 Problem Statement

Standard `msfvenom` Android payloads (e.g., `android/meterpreter/reverse_tcp`) are detected instantly by Play Protect.  
Why? Because their **signature** – including package names like `com.metasploit.stage` – is well-known to antivirus engines.

---

## 🧠 Solution

This tool performs **4‑layer hardening**:

| # | Technique | Implementation |
|---|-----------|----------------|
| 1 | **Signature Modification** | Replace `com.metasploit` and other static strings with **random package names** using `sed` |
| 2 | **Decompile & Rebuild** | Use `apktool` to decompile → modify Smali code → rebuild APK |
| 3 | **Signing** | Generate a new keystore (`keytool`) and sign the APK (`jarsigner`) → avoids *“App not installed”* error |
| 4 | **Zipalign** | Optimize APK structure (byte alignment) → looks like a normal app |

> 💡 No compiler‑level obfuscation (like Obfuscator-LLVM) is needed. We operate directly on the **APK assembly (Smali)** level.

---

## 📦 Installation

### 🔹 Kali Linux / Ubuntu

```bash
sudo apt update
sudo apt install --fix-missing
sudo apt install apktool openjdk-17-jdk zipalign
```

If openjdk-17-jdk is not found, try openjdk-21-jdk or openjdk-17-jre-headless

### Termux (Android)

```bash
pkg update
pkg install apktool openssl openjdk-17 zipalign
```

### macOS

```bash
brew install apktool
```

Also install JDK manually if not present: brew install openjdk@17

---

**Step‑by‑step after running:**

1. Enter your LHOST (local IP)
2. Enter your LPORT
3. Tool automatically:
   · Generates raw APK with msfvenom
   · Decompiles with apktool
   · Replaces all dangerous package names and strings
   · Rebuilds → signs → zipaligns
4. Output: final_obfuscated_payload.apk

---

**📁 Folder Structure (after run)**

```
apk-obfuscator/
├── original_payload.apk
├── decompiled/            # smali code
├── rebuilt_unsigned.apk
├── signed_payload.apk
├── final_obfuscated_payload.apk   ← YOUR RESULT
├── my-release-key.keystore
└── obfuscate.sh
```

---

**🧪 Example Obfuscation (Signature Mod)**

Original Replaced With
com.metasploit.stage com.x9kLpQ.a1Bc
MetasploitPayload MainActivityXyZ
/data/data/com.metasploit /data/data/com.random123

All changes are done inside Smali code using sed with regex.

---

**⚠️ Requirements**

· Java (JDK 8 or higher)
· apktool
· zipalign (Android SDK build-tools)
· msfvenom (Metasploit framework)
· keytool and jarsigner (included with JDK)

---

**❓ Troubleshooting.**

Error Solution
apktool: command not found Install via package manager (see above)
jarsigner: command not found Install JDK (openjdk-17-jdk)
zipalign not found Install zipalign or Android SDK build-tools
App not installed after sideload Run tool again – signing step may have failed

---

**📜 License**

MIT – Free for security research and authorized penetration testing only.
Do not use against devices you do not own or have explicit permission to test.

---

**✨ Author**

Built for red teamers, bug hunters, and Android security researchers.
Pull requests and obfuscation improvements are welcome.

