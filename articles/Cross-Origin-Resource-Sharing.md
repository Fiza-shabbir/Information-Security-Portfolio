#  CORS Misconfigurations

##  Introduction
While practicing web security in labs and CTF challenges, I spent significant time exploring **CORS misconfigurations**. These aren’t just theoretical risks—small mistakes in headers can expose sensitive data across domains. Through hands-on testing, I realized CORS vulnerabilities are subtle but **extremely powerful when chained with other flaws**.


##  Understanding CORS from Practice
CORS controls which domains can access an application via the browser. In practice, I found vulnerabilities mainly when:

- Access-Control-Allow-Origin reflected unvalidated origins  
- Wildcard * was used with credentials  
- Preflight (OPTIONS) requests weren’t properly validated  
- Multiple subdomains shared sensitive endpoints without proper restrictions  

##  Scenarios I Explored

###  Wildcard Origins with Credentials
- Lab scenario: An API allowed * with withCredentials: true
- Observation: Any malicious website could access a logged-in user’s data  
- Action: Created a test page to simulate requests, confirming sensitive data could be read  
- Key insight: Even if other protections exist (like CSRF tokens), wildcards + credentials can **bypass them in subtle ways**  

###  Reflective Origin Headers
- Lab scenario: The server reflected the Origin header without proper validation  
- Observation: Sending a fake origin let me read protected API responses  
- Key takeaway: One misconfigured endpoint can compromise **entire sets of sensitive endpoints**  

###  Chaining with XSS
- Lab scenario: Injected a stored XSS payload in a comment system that triggered a cross-origin request  
- Result: Stole session tokens and user-specific API responses without server compromise  
- Insight: CORS flaws often **amplify other vulnerabilities**, creating realistic multi-step attack chains  

###  Preflight & Method Abuse
- Lab scenario: OPTIONS requests allowed PUT and DELETE without proper origin checks  
- Observation: Could simulate unauthorized actions from attacker-controlled origins  
- Takeaway: Preflight misconfigurations are often **ignored in development**, but they expose critical functionality  

###  Subdomain Trust Exploit
- Scenario: An internal application trusted *.example.com in Access-Control-Allow-Origin
- Exploit: I created a malicious subdomain under the same root (e.g., attacker.example.com)  
- Result: Able to exfiltrate internal API data meant for trusted subdomains  
- Insight: Misconfigured subdomain patterns are often overlooked in large applications  

###  Sensitive Header Exposure
- Scenario: Some APIs reflected Authorization headers in responses when CORS allowed them  
- Observation: By crafting requests from attacker-controlled origins, I could access tokens or user info  
- Takeaway: Headers like Authorization or Set-Cookie require extra care in cross-origin requests  

###  Multi-Step Attack Chain
- Scenario: Combined CORS misconfiguration with CSRF and weak session handling  
- Exploit: Used attacker-controlled origin + victim’s session cookie to trigger actions like password changes  
- Result: Demonstrated how small misconfigurations **compound into high-impact exploits**  


##  Practical Workflow I Followed
1. Identify endpoints with CORS headers  
2. Check for wildcards, reflection, or subdomain patterns  
3. Test withCredentials: true for cookie/session exposure  
4. Attempt chaining with XSS, CSRF, or logic flaws  
5. Document exfiltration, unauthorized actions, or multi-step attack chains in lab reports  


##  Conclusion
Through hands on labs, I learned that Improper headers can expose authenticated data, allow unauthorized operations, and **enable multi-step attack chains** when combined with other flaws.  
