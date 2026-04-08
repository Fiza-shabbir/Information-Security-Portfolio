#  Advanced CSRF (Cross-Site Request Forgery)

## Introduction
During my web application security practice, I explored CSRF vulnerabilities across multiple labs and CTF challenges. Initially, it looked simple, but when I started testing real scenarios, I realized CSRF is more about **understanding how applications trust user sessions**.

I’ve tested different CSRF cases including form-based attacks, API abuse, and chaining with other vulnerabilities. This article summarizes the techniques I practiced and the approach I usually follow.


## 1. Basic CSRF (Form-Based Attack)

Example:
<form action="http://target.com/change-email" method="POST">
<input type="hidden" name="email" value="attacker@email.com">
</form>

- Triggers a request from victim’s browser
- Works when no CSRF protection is implemented
- I tested this in labs where applications relied only on session cookies



## 2. GET-Based CSRF

Example:
<img src="http://target.com/delete-account">

- Exploits endpoints that use GET for sensitive actions
- Very easy to exploit
- I observed many lab apps still had unsafe GET endpoints


## 3. CSRF on JSON / APIs

- Some APIs accept JSON but still rely on cookies
- I tested CSRF using fetch requests:

fetch("http://target.com/api/update", {
  method: "POST",
  credentials: "include",
  body: JSON.stringify({role: "admin"})
})

- Shows that CSRF is not limited to forms
- APIs are often overlooked


## 4. CSRF Token Bypass Techniques I Tested

### Missing Token
- No CSRF token at all → direct exploit

### Token Not Verified
- Token present but not validated on server

### Token in Cookie Only
- Double submit not properly implemented

### Same Token for All Users
- Predictable or static token → reusable


## 5. SameSite Cookie Behavior

- I tested SameSite=Lax and Strict in labs
- Observations:
  - Lax still allows some GET requests
  - Strict blocks most CSRF attempts

- Helped me understand **modern CSRF defenses**


## 6. Chaining CSRF with XSS

- Injected XSS payload to auto-submit requests
- Example:
<script>
fetch("/change-password", {
method:"POST",
credentials:"include",
body:"password=hacked"
})
</script>

- Much more powerful than standalone CSRF
- I used this in labs to simulate full account takeover


## 7. Multi-Step CSRF Attack

- Some applications required multiple steps
- I chained multiple requests in sequence
- Example:
  1. Change email
  2. Trigger password reset
  3. Take over account

- This taught me CSRF is often part of a **bigger attack chain**


## My Testing Workflow

When testing CSRF, I usually follow:

1. Identify sensitive actions (change password, email, delete account)
2. Check if CSRF token exists
3. Remove or modify token
4. Create PoC (HTML form or script)
5. Test from another origin
6. Observe if action succeeds


## Key Learnings

- CSRF is not about payloads, it’s about **session trust**
- APIs are often vulnerable even when forms are protected
- SameSite cookies reduce risk but don’t eliminate it
- Chaining CSRF with XSS or logic flaws increases impact



## Conclusion

From my practice, CSRF is one of those vulnerabilities that looks simple but becomes powerful in real scenarios. Understanding how browsers handle cookies and how applications trust requests is key to exploiting and preventing CSRF effectively.
