# Broken Access Control 

**Scope:** Exploitation only. 
**What it is:** Any flaw that lets a user perform actions or access data outside their intended permissions (e.g., unauthenticated admin panels, role flags in cookies/params, IDORs, proxy/header trust issues).

---

## Attack Surface & Recon 
- **Enumerate sensitive paths:** `robots.txt`, directory brute-force for `/admin`, `/internal`, hidden panels.  
- **Client hints:** View Source / Inspect Elements to catch hardcoded admin links or JS routes. Browsers render HTML/JS; everything you see is text—inspect it.  
- **State/role carriers:** Cookies and request parameters (`admin=true`, `role=admin`, `isAdmin=0/1`, `user=alice`).  
- **Horizontal privilege escalation:** Identify other users, then request their objects (profiles, orders, messages) by changing IDs or names.  
- **Proxy/front-door bypass:** Look for reverse-proxy headers (e.g., `X-Original-URL`) or direct path requests that skip UI checks.
- **Tooling setup:** Burp Proxy + Repeater; **Options → Match & Replace** to auto-flip recurring role flags; feroxbuster for path discovery.

---

## Labs Covered (scenario variants)

### 1) Unprotected admin functionality
**Technique:** Robots + direct panel access  
**Minimal repro:**
1. `GET /robots.txt` → note disallowed admin path.  
2. `GET /admin` → reach admin UI; invoke dangerous action (e.g., delete user).  


---

### 2) Unprotected admin functionality with unpredictable URL
**Technique:** Source inspection to discover hidden admin route  
**Minimal repro:**
1. **View Source / Inspect** on main page → locate admin link/JS route embedded in HTML/JS.  
2. Request that path directly → admin UI available; perform privileged action.  


---

### 3) User role controlled by request parameter (cookie/param tampering)
**Technique:** Flip role flag, automate with Match & Replace  
**Minimal repro:**
1. Intercept a request in Burp; observe `admin=false` (cookie or param).  
2. Set `admin=true`; forward → privileged endpoint becomes usable.  
3. Add **Match & Replace** rule so every request keeps `admin=true`.  


---

### 4) User ID controlled by request parameter with data leakage in redirect
**Technique:** IDOR + redirect-based leakage  
**Minimal repro:**
1. Send request for current user’s resource (e.g., `/my-account?id=alice`).  
2. Change `id` to another user (e.g., `carlos`).  
3. Even if redirected/denied, inspect the intermediate response/HTML for leaked details (names, emails, order IDs).  


---

### 5) URL-based access control can be circumvented
**Technique:** Direct path + proxy header influence  
**Minimal repro:**
1. Discover `/admin` via feroxbuster; UI says “Access denied,” but the path exists.  
2. Repeater: add header `X-Original-URL: /admin` (or directly request the internal path if routing trusts it).  
3. Re-send → backend routes you to admin context; perform privileged action.  


> Note on `X-Original-URL`: If the app/proxy trusts client-supplied routing headers, you can steer requests to internal routes, bypassing front-end checks and confusing caches/ACLs.

---

## Tools I used
- **Burp Suite** (Proxy, Repeater; Options → Match & Replace for persistent cookie/param flips).  
- **feroxbuster** for fast path discovery and status-code patterning.  
- **Browser DevTools** (View Source / Inspect) to find embedded admin links and client-side routes.


- **Cache/redirect gotchas:** examine intermediate responses, not just the final 302/200.  
- **Parameter naming drift:** try variants (`role`, `isAdmin`, `admin`, `priv`, `tier`, `user`, `uid`, `id`).
