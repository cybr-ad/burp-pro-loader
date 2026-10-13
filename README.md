# Burp Pro Loader

[![Java](https://img.shields.io/badge/Java-17%2B-orange.svg)](https://openjdk.java.net/)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-blue.svg)]()
[![Burp Suite](https://img.shields.io/badge/Burp%20Suite-Professional-red.svg)](https://portswigger.net/burp)
[![License](https://img.shields.io/badge/License-Educational%20Only-yellow.svg)]()

> **A Java agent–based loader for Burp Suite Professional that enables Burp Suite Pro to start without a valid license key by patching license validation at runtime.**

---

## ⚠️ Disclaimer

> **This project is for educational and research purposes only.**  
> Use it only in environments where you have explicit permission.  
> The author is **not responsible** for any misuse or legal consequences.  
> You are solely responsible for complying with all applicable laws and the Burp Suite license agreement.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| **JDK Support** | Works with JDK 17 – 25 (fully supported) |
| **JDK 26** | Partially supported (see [Known Issues](#-known-issues)) |
| **Cross-Platform** | Windows, Linux, macOS |
| **Non-Invasive** | Does not modify the original Burp Suite JAR |
| **Simple Launch** | One-line command to start |
| **Prebuilt JAR** | Ready-to-use `burploader.jar` available |

---

## 📋 JDK Compatibility

| JDK Version | Status | Notes |
|-------------|--------|-------|
| **JDK 17 – 25** | ✅ **Fully Supported** | Recommended for production use |
| **JDK 26** | ⚠️ **Partial** | May have issues; see workarounds below |

> **💡 Recommendation:** Use **JDK 21** for the best stability and compatibility.  
> **💡 Pro Tip:** For best stability, use a JDK that is 1–2 versions behind the latest release. Brand-new JDK releases may have compatibility quirks. JDK 17 is the long-term support (LTS) baseline; the JAR runs on 17 through 26.

---

## ⚙️ Requirements

- **Java Development Kit (JDK) 17 or newer** — [Download JDK 21 (Recommended)](https://adoptium.net/temurin/releases/?version=21)
- **Burp Suite Professional JAR** — Download from [PortSwigger](https://portswigger.net/burp/releases/professional-community-2024-6)
- **`burploader.jar`** — From this repository

---

## 📦 Installation

### 1. Get the Loader

**Option A: Clone the Repository**
```bash
git clone https://github.com/cybr-ad/burp-pro-loader.git
cd burp-pro-loader
```

**Option B: Direct Download**  
Download `burploader.jar` from the [Releases](https://github.com/cybr-ad/burp-pro-loader/releases) page.

### 2. Download Burp Suite Professional

Download the official Burp Suite Professional JAR from [PortSwigger](https://portswigger.net/burp/releases/professional-community-2024-6).

### 3. Organize Files

Place both JAR files in the same directory:

```
burp-pro-loader/
├── burploader.jar
└── burpsuite_pro_v2026.6.jar   ← Your Burp Suite JAR (name may vary)
```

---

## 🚀 Usage

### Basic Command

```bash
java -javaagent:burploader.jar -jar burpsuite_pro_v2026.6.jar
```

> **Important:** Replace `burpsuite_pro_v2026.6.jar` with the actual filename of your Burp Suite Professional JAR.

### Platform-Specific Examples

#### Windows (Command Prompt)
```cmd
java -javaagent:burploader.jar -jar burpsuite_pro_v2026.6.jar
```

#### Linux / macOS
```bash
java -javaagent:burploader.jar -jar burpsuite_pro_v2026.6.jar
```

---

## 📝 Launcher Scripts (Optional)

### Linux / macOS — `run_burp.sh`
```bash
#!/bin/bash
cd /path/to/burp-folder
java -javaagent:burploader.jar -jar burpsuite_pro_v2026.6.jar
```
Make it executable:
```bash
chmod +x run_burp.sh
./run_burp.sh
```

### Windows — `run_burp.bat`
```bat
@echo off
cd /d "C:\path\to\burp-folder"
java -javaagent:burploader.jar -jar burpsuite_pro_v2026.6.jar
```

---

## ⚠️ Known Issues

### JDK 26 Compatibility

JDK 26 introduces changes to the internal `jdk.internal.org.objectweb.asm` package that the loader depends on. This may cause:
- `NoSuchMethodError`
- `IllegalAccessError`
- Loader failing to start

#### Workarounds

| Workaround | Description |
|------------|-------------|
| **Use JDK 21** | Most reliable option. Install JDK 21 and point `java` to it. |
| **Use JDK 17–25** | Any version in this range works without issues. |
| **JDK 26 with Flags** | Try adding these JVM flags (may not resolve all issues): |

```bash
java --add-opens java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED \
     -javaagent:burploader.jar -jar burpsuite_pro_v2026.6.jar
```

---

## 🛠️ Troubleshooting

| Problem | Solution |
|---------|----------|
| `Could not find or load main class` | Verify the Burp Suite JAR filename is correct and the file exists. |
| `UnsupportedClassVersionError` | Your Java version is too old. Install **JDK 17 or newer**. |
| Burp starts but shows license error | Ensure `-javaagent:burploader.jar` is placed **before** `-jar`. |
| Loader works but Burp crashes | Try **JDK 21** — it's the most stable version for this loader. |
| `Permission denied` on Linux/macOS | Run `chmod +x run_burp.sh` or use `sudo` if needed. |

---

## ❓ FAQ

<details>
<summary><strong>Does this modify the Burp Suite JAR?</strong></summary>
No. It uses a Java agent to patch license validation at runtime. The original JAR remains unchanged.
</details>

<details>
<summary><strong>Which Burp Suite versions are supported?</strong></summary>
Recent Burp Suite Professional releases (2024.x – 2026.x). If a new Burp version breaks compatibility, check for an updated `burploader.jar`.
</details>

<details>
<summary><strong>Do I need JDK 26?</strong></summary>
No. **JDK 21 is recommended**. JDK 17–25 are fully supported.
</details>

<details>
<summary><strong>Is this legal?</strong></summary>
This project is for educational and research purposes only. You are responsible for complying with all applicable laws and the Burp Suite license agreement.
</details>

<details>
<summary><strong>Why does the loader fail on JDK 26?</strong></summary>
JDK 26 changed internal ASM classes (`jdk.internal.org.objectweb.asm`) that the loader relies on for bytecode manipulation.
</details>

---

## 📁 Repository Structure

```
burp-pro-loader/
├── README.md          # This file
└── burploader.jar     # The loader JAR (prebuilt)
```

---

## 🔧 Technical Details

The loader operates as a **Java Agent** using the `java.lang.instrument` API:

1. **Bytecode Instrumentation** — Intercepts class loading at runtime
2. **ASM-Based Transformation** — Uses internal JDK ASM classes for class transformation
3. **LicenseManager Injection** — Injects a custom `LicenseManager` that always returns a valid license state
4. **Non-Persistent** — All changes happen in memory; the original JAR is never modified

---

## 🙏 Credits

- **[cybr-ad](https://github.com/cybr-ad)** — Project author & maintainer
- **[PortSwigger](https://portswigger.net/)** — Burp Suite

> This project is **not affiliated with or endorsed by PortSwigger**.

---

## 📄 License

This project is provided for **educational purposes only**.  
No commercial license is granted. Use at your own risk.

---

## 🌟 Support the Project

If you find this useful, consider:
- ⭐ Starring the repository
- 🐛 Reporting issues
- 🔀 Contributing improvements

---

*Made with ❤️ for the security research community*
