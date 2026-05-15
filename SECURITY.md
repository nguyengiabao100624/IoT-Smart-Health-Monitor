# Security Policy

## Supported Versions

Currently, only the latest `main` branch is actively supported with security updates.

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x   | :x:                |
| Main    | :white_check_mark: |

## Reporting a Vulnerability

Security is a high priority for this project. If you discover a security vulnerability, please DO NOT report it by opening a public issue.

Instead, please send an email to the project maintainer privately. We will make every effort to acknowledge your report within 48 hours and will provide a status update as we investigate and resolve the issue.

### Safe Practices
1. **Never commit `secrets.h`**: Ensure that your Wi-Fi credentials and Firebase API keys are stored in `include/secrets.h` and that this file remains in `.gitignore`.
2. **OTA Security**: If using OTA (Over-The-Air) updates, ensure you use a strong password for `upload_flags = --auth=your_strong_password`.
