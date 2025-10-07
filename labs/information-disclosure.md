# Information Disclosure 

**Scope:** Exploitation only. 
**What it is:** Exposing data that shouldn’t be accessible (stack traces, source/backup files, version banners, internal paths, credentials, etc.). 

---

## Attack Surface & Recon 

- **Discover hidden paths**
  - Check `robots.txt` and follow listed disallowed paths. This is a common starting point for hidden content discovery. 
  - There’s no guaranteed way to enumerate “all” hidden paths; brute-force with a wordlist is the practical route. 

- **Directory/content discovery**
  - Use **feroxbuster** to enumerate files/dirs that often hide sensitive artifacts.  
    `feroxbuster -u https://TARGET -w /path/to/wordlist` 
  - Read HTTP codes in context to triage interesting hits (200/300/400 patterns). 

- **Exposed version control**
  - Look for `/.git/` exposure; if accessible, mirror it and inspect history for secrets. 

- **Verbose error paths**
  - Intentionally trigger malformed inputs and review server errors for DB structure, file paths, versions. 

---

## Labs Covered (exploitation steps)

### 1) Source Code Disclosure via Backup Files
**Goal:** Prove source exposure by retrieving backup or leftover files.

**Method (minimal repro):**
1. Fetch `https://TARGET/robots.txt` and walk any disallowed paths it reveals. 
2. Run content discovery to surface stray backups/snapshots.  
   `feroxbuster -u https://TARGET -w /path/to/wordlist` 
3. Request candidate backup names surfaced by discovery.  
4. Save the raw response as evidence (trim/redact any live secrets before committing notes).  
   *Notes:* Hidden paths aren’t guaranteed in `robots.txt`; brute-force is expected. 


---

### 2) Information Disclosure via Version Control History (`.git`)
**Goal:** Recover sensitive data from exposed Git metadata.

**Method (minimal repro):**
1. Confirm access to `/.git/HEAD`. If reachable, mirror the repo:  
   `wget -r https://TARGET/.git/` 
2. Reconstruct/open the repository locally and inspect commit history/diffs for secrets (credentials, tokens, config keys). 

---

### 3) Information Disclosure in Error Messages
**Goal:** Extract sensitive environment details via server-generated errors.

**Method (minimal repro):**
1. Send inputs designed to break assumptions (oversized values, wrong types, malformed structures). 
2. Record server responses that leak stack traces, absolute paths, DB engine/version, or framework banners. 


---

## Tools I used

- **feroxbuster** for path discovery with a sane wordlist. 
- **wget** (recursive) to mirror exposed `.git` trees for offline analysis. 
- **Burp (Proxy/Repeater)** to generate and observe error conditions and responses. 

---

## Notes & Pitfalls

- Hidden content isn’t always indexed or advertised; plan for enumeration time. 
- Treat HTTP status code patterns as hints, not gospel—follow up with manual inspection. 



