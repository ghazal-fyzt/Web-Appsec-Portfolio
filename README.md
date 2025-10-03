# Web AppSec Portfolio — PortSwigger Labs

This repository documents my hands-on journey through PortSwigger Web Security Academy. Each write-up shows how I approached the problem: my reasoning, payloads, verification steps, and quick “how to fix” notes. I treat these as reproducible experiments rather than one-off spoilers.

## What’s inside

- **labs/** — One markdown per vulnerability. 
- **tools/** — Short, practical notes for the tools I actually used.
- **playbooks/**  
  - `web-appsec-playbook.md` — My personal, living **bug-hunting checklist** for real targets (recon to report). This is the distilled “map” I built after the labs.
- **templates/** — Reusable templates for new lab write-ups and bug reports.

> I keep solutions minimal but reproducible. If a step isn’t necessary to prove impact, it doesn’t go in.

## Skills & focus areas

- **HTTP & proxying:** precise request/response control with Burp (Repeater, Intruder, Collaborator).
- **Access control:** IDOR, horizontal/vertical privilege escalation, cookie/state tampering.
- **Client-side:** reflected/stored/DOM XSS, CSP bypass techniques, HTML injection contexts.
- **Injection:** SQLi (in-band/UNION/blind/time-based), OS command injection (incl. blind), XXE.
- **Server-side pivoting:** SSRF (local/remote, blacklist/whitelist bypass, open-redirect chaining).
- **Discovery & traversal:** `robots.txt`, exposed VCS (`.git`), directory traversal, wordlists, content discovery with feroxbuster.
- **Auth flows:** CSRF logic errors, OAuth misconfigurations (redirect URI, linking flows).

## How to read this repo

1. Start with **`playbooks/web-appsec-playbook.md`** to see the real-world testing flow I follow now.
2. Dive into **`labs/`** for concrete reproductions and prevention tips.
3. Skim **`tools/`** when you want exact commands or Burp configs.

## Ethics & scope

All content here is for learning and lawful testing only. 

