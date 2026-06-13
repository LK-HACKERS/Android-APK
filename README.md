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
