---
title: mat3uszm **SAST**. Static Application Security Testing, Automating the Fight Against Vulnerabilities
layout: default
---

# Static Application Security Testing (SAST): Automating the Fight Against Vulnerabilities

## Intro

The cybersecurity landscape has evolved drastically, and the stakes for application security have never been higher. Nowadays, waiting until a post-release audit to identify vulnerabilities is a luxury we simply cannot afford. I’ve become a strong advocate for the 'Shift Left' philosophy—the practice of addressing security risks at the earliest possible stage: the codebase itself. This is where SAST (Static Application Security Testing) becomes a game-changer, allowing us to detect critical flaws and configuration errors during the initial development phase, long before the application ever goes live.

## Static Application Security Testing

Static Application Security Testing (SAST) is a white-box testing methodology that analyzes the application's source code, byte code, or binaries without executing the program. It relies on a set of predefined rules or semantic patterns to identify structural vulnerabilities, logic flaws, and non-compliance with security standards. By mapping the data flow and control paths within the codebase, SAST provides a comprehensive overview of the application's security posture at rest.
In simpler, engineering terms, it’s like having an automated, ultra-fast peer reviewer who has memorized every security handbook ever written. While a human might skim over a hardcoded IP address or a risky 'eval()' call during a Friday afternoon code review, SAST never blinks. It treats your code as data, scanning for dangerous patterns before they ever have a chance to become a running threat. It’s the ultimate 'sanity check' that turns security from a manual chore into a scalable, automated process.

## From Theory to Action – Why Semgrep?

To bring this concept to life, I chose Semgrep as my primary SAST engine. What sets it apart is its incredible flexibility and speed, making it perfect for modern CI/CD pipelines. One of the greatest strengths of this setup is the ability to combine global standards with project-specific logic. I’ve leveraged a mix of 'battle-tested' community rulesets for general security (like OWASP Top 10) and my own custom-defined rules tailored to the application's unique architecture. Whether it's enforcing specific networking protocols or preventing the use of risky internal functions, the ability to write custom YAML-based rules means the security gate evolves alongside the code. This hybrid approach ensures we aren't just ticking a compliance box—we are building a bespoke defense system.

## The Implementation – Real-World Rules

Theory is good, but code is better. To make this security gate a reality, I’ve implemented a series of custom Semgrep rules that target the specific 'pain points' of our application. Below is a glimpse of my configuration—a modular setup where I’ve separated concerns into different categories. One of the most critical rules I've written focuses on preventing secret leakage by scanning for high-entropy strings and known API key patterns, ensuring that no sensitive credentials ever make it into the repository.
The following automation example is specifically tailored for a React Native application, integrated seamlessly via GitHub Actions to handle the unique challenges of mobile app security.

### Script to run Semgrep in GitHub Actions

```
name: Security & License Scan
on:
  push:
    branches:
      - main
      - develop
  pull_request:
    branches:
      - main
      - develop

jobs:
  static-analysis:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Semgrep (SAST)
        run: |
          python3 -m pip install semgrep
          semgrep scan --config p/react --config p/javascript --config p/default --config p/secrets --config .github/semgrep_rules/ --text > sast_report.txt
          # semgrep scan --config p/react --config p/javascript --config p/default --config p/secrets --config .github/semgrep_rules/ --json --output sast_report.json
      
      - name: Force Fail if secrets found
        run: |
          if grep -q "CRITICAL" sast_report.txt; then
            echo "-------------------------------------------------------"
            cat sast_report.txt
            echo "-------------------------------------------------------"
            echo "Security check failed!"
            exit 1
          fi

      - name: Archive Security Reports
        uses: actions/upload-artifact@v4
        if: always()
        with:
          name: security-reports
          path: |
            sast_report.txt
            # sast_report.json
```

### Rules examples

#### Detect Hardcoded Credentials

```
rules:
  - id: hardcoded-secrets-blocking
    message: "CRITICAL: Secret detected! Remove it from code before merge and REVOKE the key immediately (it is now compromised)."
    severity: ERROR
    languages: [generic]
    patterns:
      - pattern-either:
          # Google API Key
          - pattern-regex: 'AIza[0-9A-Za-z-_]{35}'
          # AWS Access Key
          - pattern-regex: 'AKIA[0-9A-Z]{16}'
          # Stripe Secret Key
          - pattern-regex: 'sk_live_[0-9a-zA-Z]{24}'
          # Firebase API Key
          - pattern-regex: 'AIzaSy[0-9A-Za-z-_]{33}'
          # Slack Webhook
          - pattern-regex: 'https://hooks.slack.com/services/T[a-zA-Z0-9_]+/B[a-zA-Z0-9_]+/[a-zA-Z0-9_]+'
          # Basic passwords
          - pattern-regex: '(?i)(password|secret|token|api_key|apikey)\s*[:=]\s*["''][a-zA-Z0-9!@#$%^&*()_+]{16,}["'']'
```

#### Detect hardcoded IP addresses

```
rules:
  - id: no-hardcoded-ips
    message: "WARNING: Hardcoded IP address detected. Use DNS names or environment variables."
    severity: ERROR
    languages: [generic]
    pattern-regex: '\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b'
```

#### Detect not secure connection

```
rules:
  - id: no-http-allowed
    message: "NETWORKING ERROR: Using http:// is strictly forbidden. Use https:// to prevent Man-in-the-Middle attacks."
    severity: ERROR
    languages: [generic]
    pattern-regex: "http://[a-z0-9.-]+"
```

#### Detect console.log

```
rules:
  - id: no-console-log-production
    message: "CODE QUALITY: Remove console.log before pushing. It can leak sensitive data in production logs."
    severity: ERROR
    languages: [javascript, typescript]
    pattern: console.log(...)
```

#### Detect not secure webview 

```
rules:
  - id: risky-html-rendering
    message: "WARNING: Dangerous HTML rendering detected. Ensure content is sanitized!"
    severity: ERROR
    languages: [javascript, typescript]
    pattern-either:
      - pattern: dangerouslySetInnerHTML={...}
      - pattern: "({html: ...})"
```

#### Detect not secure React Native storage


```
rules:
  - id: no-sensitive-data-in-asyncstorage
    message: "WARNING: AsyncStorage is not encrypted. Use react-native-keychain or react-native-encrypted-storage for tokens and secrets."
    severity: ERROR
    languages: [javascript, typescript]
    pattern-regex: 'AsyncStorage\.setItem\(.*(token|auth|password|secret|jwt).*\)'
```

## Summary: Peace of Mind as a Priority

The ultimate benefit of this setup isn't just a clean report—it's the peace of mind. By automating the "boring" but critical security checks, I’ve built a professional safety net that allows development teams to focus on delivering value without the constant fear of accidental leaks.
Whether I'm securing a complex React Native frontend or a robust backend API, the principle remains the same: Application Security must be invisible, automated, and proactive. Implementing this Static Application Security Testing (SAST) pipeline is a one-time investment that pays dividends across the entire software development lifecycle (SDLC).
In today's landscape, being a Shift-Left Advocate is more than just a title—it's about building a culture where security is a shared responsibility, backed by reliable automation.

