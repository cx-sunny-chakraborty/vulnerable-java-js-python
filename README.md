# CxOne Language Mode Demo

A small intentionally-vulnerable polyglot project for demonstrating the **SAST Language Mode** parameter in Checkmarx One.

> ⚠️ **WARNING:** This repository contains intentionally vulnerable code for security training and demonstration purposes only. **Do not deploy or use in production.**

## Purpose

This project demonstrates how the `languageMode` setting in CxOne SAST affects scan results when a repository contains multiple **primary languages**.

## Project Structure

```
.
├── java-app/           # Java code (dominant primary language)
│   └── src/main/java/com/demo/
│       ├── UserController.java    # SQL Injection
│       ├── FileHandler.java       # Command Injection, Path Traversal
│       ├── SearchServlet.java     # XSS, Open Redirect
│       └── CryptoUtils.java       # Hardcoded secrets, weak crypto
│
├── python-app/         # Python code (secondary primary language)
│   ├── app.py          # SQL Injection, Command Injection
│   └── utils.py        # Path Traversal, Insecure Deserialization
│
└── frontend/           # JavaScript (Java's secondary language)
    ├── search.js       # DOM XSS, Open Redirect, eval
    └── index.html
```

## File Count Summary

| Language | Files | Role |
|----------|-------|------|
| Java | 4 | Dominant primary language |
| Python | 2 | Other primary language |
| JavaScript | 1 | Java's secondary language |
| HTML | 1 | Static asset |

Java has the highest file count, so it becomes the **dominant primary language**.

## Expected Scan Behavior

### Primary Mode (`languageMode: 'primary'`)
- ✅ Scans **Java** (dominant primary)
- ✅ Scans **JavaScript** (Java's secondary language)
- ❌ **Skips Python** entirely

### Multi Mode (`languageMode: 'multi'`)
- ✅ Scans **Java**
- ✅ Scans **Python**
- ✅ Scans **JavaScript**
- All primary languages + all their secondaries

## Vulnerabilities Included

### Java
- SQL Injection
- Command Injection
- Path Traversal
- Reflected XSS
- Open Redirect
- Hardcoded Secrets
- Weak Cryptography (MD5, DES)

### Python
- SQL Injection
- Command Injection
- Path Traversal
- Insecure Deserialization (pickle)
- Hardcoded Secrets
- Flask Debug Mode

### JavaScript
- DOM-based XSS
- Open Redirect
- `eval()` with user input

## Running the Demo

1. Fork or clone this repository
2. Connect it to a Checkmarx One project
3. Create two project rules — one with `languageMode = multi`, one with `languageMode = primary`
4. **Important:** Disable `Recommended Exclusions` on both projects for a clean comparison
5. **Important:** Disable `Fast Scan Mode` at the tenant level (otherwise it forces primary)
6. Trigger scans and compare results

## Reference

- [Checkmarx One — Specifying a Code Language for Scanning](https://docs.checkmarx.com/)
- [Config as Code Documentation](https://docs.checkmarx.com/)
