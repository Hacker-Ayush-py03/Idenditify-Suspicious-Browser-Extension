# Idenditify-Suspicious-Browser-Extension
# 🔍 Identify & Remove Suspicious Browser Extensions
**Project**: Browser Extension Audit & Removal  
**Goal**: Help users and admins identify, document and safely remove suspicious or unnecessary browser extensions (Chrome / Chromium, Firefox).

---

## 🚀 Overview (सार)
Browser extensions increase browser功能 but also expand attack surface. This repository provides:
- A clear audit workflow
- Permission & risk checklist
- Manual removal steps for common browsers
- Scripts to list/backup extensions (Linux/macOS/Windows—use with caution)
- Incident reporting template & remediation checklist
- Interview Q&A and security best practices

---

## 📋 Table of Contents
- [When to run this audit](#-when-to-run-this-audit)
- [Threat model & risks](#-threat-model--risks)
- [Audit workflow (detailed)](#-audit-workflow-detailed)
- [Manual steps — Chrome/Chromium](#-manual-steps--chromechromium)
- [Manual steps — Firefox](#-manual-steps--firefox)
- [Scripts (list / backup / remove) — Examples](#-scripts-list--backup--remove---examples)
- [Documentation / Deliverables](#-documentation--deliverables)
- [Incident report template](#-incident-report-template)
- [Interview Q&A (common questions + answers)](#-interview-qa-common-questions--answers)
- [Security best practices](#-security-best-practices)
- [Resources & references](#-resources--references)
- [License](#-license)

---

## 🕒 When to run this audit
- New machine provisioning
- After suspicious browser behavior (ads, redirects, credential prompts)
- Regular security hygiene (quarterly)
- Before/after handing over a dev/test machine

---

## ⚠️ Threat model & risks
Extensions can:
- Intercept or modify web requests (steal tokens, inject scripts)
- Exfiltrate browsing history, credentials, form data
- Install persistent trackers or adware
- Run cryptomining in background
- Escalate via vulnerable native messaging hosts

---

## 🛠 Audit workflow (detailed)
1. **Inventory**: enumerate all extensions across browsers.
2. **Permissions review**: note high-risk permissions (e.g., `read and change all data on websites`).
3. **Reputation check**: publisher, store reviews, source of .crx/.xpi.
4. **Behavioral checks**: network requests, CPU/memory spike, new native hosts.
5. **Isolate & disable** suspicious ones first (don't remove immediately in high-risk incidents).
6. **Backup** extension data and browser profile.
7. **Remove** confirmed malicious/unneeded extensions.
8. **Restart** browser; monitor for changes.
9. **Document** steps, evidences, and update deliverables.

---

## 🔎 What permissions should raise suspicion?
- Access to *all* websites / "read and change all your data on the websites you visit"
- Capture keystrokes or tabs
- Native messaging host access
- Cross-origin or wide network access
- Unusually broad host permissions in Firefox (e.g., `*` patterns)

---

## 🧭 Manual steps — Chrome / Chromium (Windows, macOS, Linux)
**Open extensions page:**
- Chrome: `chrome://extensions/`
- Edge (Chromium): `edge://extensions/`

**Review each extension:**
- Publisher name, website link, version, last updated
- Click **Details** → check permissions, ext origin, extension ID

**Disable first, then remove:**
1. Toggle off suspicious extension.
2. Use browser for a while; if behavior improves, **Remove**.
3. Optionally, clear browser cache and restart.

---

## 🧭 Manual steps — Firefox
**Open add-ons manager:**
- `about:addons`

**Review:** Select extension → More → check permissions + homepage + support link.

**Disable → Monitor → Remove** if malicious.

---

## 🧰 Scripts — list / backup / remove (examples)
> **Important:** Scripts are examples. Test in safe environment and create backups. Some paths differ by OS / profile name.

### 1) List Chrome/Chromium extensions (Linux/macOS)
```bash
#!/usr/bin/env bash
# list-chrome-extensions.sh
# finds Chrome/Chromium extension directories for default profile
PROFILE_DIRS=("$HOME/.config/google-chrome/Default/Extensions" "$HOME/.config/chromium/Default/Extensions" "$HOME/Library/Application Support/Google/Chrome/Default/Extensions")
for d in "${PROFILE_DIRS[@]}"; do
  if [ -d "$d" ]; then
    echo "Extensions in: $d"
    ls -1 "$d" || true
  fi
done
```
### 2) Backup Chrome extensions (pack folders)
```bash
#!/usr/bin/env bash
# backup-chrome-extensions.sh
OUT="$HOME/chrome_extensions_backup_$(date +%F_%T)"
mkdir -p "$OUT"
PROFILE_DIR="$HOME/.config/google-chrome/Default/Extensions"
if [ -d "$PROFILE_DIR" ]; then
  cp -r "$PROFILE_DIR" "$OUT/"
  echo "Backed up to $OUT"
else
  echo "Profile directory not found: $PROFILE_DIR"
fi
```
### 3) Remove a Chrome extension (by folder/ID)
```bash
# Danger: destructive. Use only after confirming ID and backup.
rm -rf "$HOME/.config/google-chrome/Default/Extensions/<EXTENSION_ID>"
# In Windows the extensions are in %LOCALAPPDATA%\Google\Chrome\User Data\Default\Extensions
```
### 4) List Firefox extensions (Linux/macOS)
```bash
#!/usr/bin/env bash
FIREFOX_PROFILES="$HOME/.mozilla/firefox"
if [ -d "$FIREFOX_PROFILES" ]; then
  for prof in "$FIREFOX_PROFILES"/*.default*; do
    if [ -d "$prof" ]; then
      echo "Profile: $prof"
      grep -E '"id"|name' "$prof"/extensions.json || true
    fi
  done
fi
```
# 🧾 Documentation / Deliverables

* For each audit, deliver:

    * audit-report.md (summary)

      * Date, operator, machine ID

      * Inventory table (browser, extension name, ID, permissions, source, action taken)

      * Evidence (screenshots, log snippets)

      * Final status (removed/disabled/kept) and justification


   * Backup archive (if any)

   * Incident timeline (if suspicious)


# ***_Sample inventory table (markdown):_***

|Browser	|Extension	ID	|Permissions	|Risk level	|Action|
|---------|---------------|-------------|-----------|------|
|Chrome	|AwesomeExt|	abcdefgh	|read/change all sites|	High|	Removed (2025-05-01)|


# 📝 Incident report template


# Incident Report: Suspicious Browser Extension
- **Date/Time:** YYYY-MM-DD HH:MM
- **Reported by:** <name>
- **Machine / Hostname:** <host>
- **Browser & Version:** Chrome 123.0 / Firefox 120.x
- **Extension(s) involved:** name (ID)
- **Observed behavior:** redirects, popups, exfil attempts, CPU spike, etc.
- **Actions taken:** disabled -> captured network logs -> backed up profile -> removed
- **Artifacts collected:** screenshot(s), extension folder backup, browser logs
- **Root cause / Notes:** e.g., “came bundled with installer X”
- **Remediation & follow-up:** password change, MFA enforcement, full AV scan, notify stakeholders
- **Report owner:** <name>

# ✅ Security best practices (quick)

* Limit global host permissions; prefer per-site grants.

* Use enterprise policies for managed environments to whitelist/blacklist extensions.

* Enforce MFA & rotate credentials if extension was malicious.

* Monitor browser telemetry (network calls, CPU, memory).

* Backup profiles before removal for forensic needs.



---

# 🧪 Testing & Validation

* After removal, verify:

   * No suspicious network requests in DevTools Network tab

   * Browser performance restored

   * No reinstallation attempts or persistence

 
 * If evidence of compromise: treat as incident, isolate host.


# 👋 Final notes 

* Always backup before removing anything.

* Test scripts carefully and adapt paths for your OS/profile.

* If you want, I can also:

* produce a powershell variant for Windows,

* create a small UI checklist (HTML) or a GitHub Actions workflow to enforce extension policy for managed profiles.
