#  Advanced SSRF (Server-Side Request Forgery)

## Introduction
While working on labs and CTF challenges, I explored SSRF vulnerabilities in different scenarios. Unlike client-side attacks, SSRF focuses on how the **server processes user-controlled URLs**.

Through practice, I learned how SSRF can be used not only to fetch external resources but also to **interact with internal services and hidden endpoints**.


## 1. Basic SSRF

Example:
http://target.com/fetch?url=http://example.com

- Server fetches user-supplied URL
- I tested by replacing with my own controlled endpoint


## 2. Internal Network Access

Example:
http://target.com/fetch?url=http://127.0.0.1:8080

- Accessing internal services
- I discovered hidden admin panels in labs


## 3. Localhost Bypass Techniques

- 127.0.0.1
- localhost
- 0.0.0.0
- 0177.0.0.1 (octal)

- I used these variations to bypass filters
- Many filters only block exact matches

---

## 4. Accessing Cloud Metadata

Example:
http://169.254.169.254/latest/meta-data/

- Used in cloud environments
- I tested this in labs simulating AWS

- Retrieved:
  - Instance info
  - Credentials (in lab setups)

## 5. SSRF via File Upload / Image Fetch

- Some apps fetch images from URLs
- I replaced image URL with internal endpoint

- Example:
http://target.com/upload?image=http://127.0.0.1/admin

- Helped me understand indirect SSRF vectors


## 6. Blind SSRF

- No response returned
- I used external logging servers to confirm requests

Example:
http://attacker.com/log

- If request hits my server → SSRF confirmed


## 7. Protocol Exploitation

- Tested different protocols:
  - http://
  - https://
  - file://
  - ftp://

- Some apps allowed unexpected protocols
- This expanded attack surface

---

## 8. Filter Bypass Techniques

- URL encoding
- Double encoding
- Redirect chains
- Using subdomains

Example:
http://trusted.com@127.0.0.1

- Helped bypass naive validations

## 9. Chaining SSRF with Other Vulnerabilities

- Combined SSRF with open redirect
- Used SSRF to trigger internal APIs
- In some labs, SSRF led to **privilege escalation scenarios**


## My Testing Workflow

1. Identify URL-based inputs
2. Replace with external URL (test server)
3. Try internal addresses (127.0.0.1, localhost)
4. Attempt metadata access
5. Test bypass techniques
6. Observe responses or logs


## Key Learnings

- SSRF is about **server trust in user input**
- Internal services are often exposed
- Filters are usually weak and bypassable
- Blind SSRF requires indirect verification


## Conclusion

From my experience, SSRF is a powerful vulnerability that can expose internal systems and sensitive data. It requires patience and testing multiple variations, but once exploited, it can lead to serious impact in real-world environments.
