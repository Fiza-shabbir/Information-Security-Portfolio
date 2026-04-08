## 🧩 Real-World Broken Access Control Scenarios I Explored

### 🛒 1. E-Commerce Price Manipulation

**Scenario:**
While testing a lab-based e-commerce application, I intercepted a purchase request:

POST /checkout  
{
  "product_id": 101,
  "price": 500
}

👉 By modifying the request:
{
  "product_id": 101,
  "price": 1
}

**Outcome:**
- The application accepted the modified price  
- Allowed purchasing products at a lower cost  

**Learning:**
- Critical logic must be validated server-side  
- Never trust client-side values for sensitive operations  

---

### 👤 2. Accessing Other User Profiles (IDOR)

**Scenario:**
/profile?id=102  

👉 Changed to:
/profile?id=101  

**Outcome:**
- Accessed another user’s personal information  
- No authorization check was enforced  

**Advanced Observation:**
- IDs were sequential → allowed enumeration of multiple users  

---

### 🔑 3. Password Reset Token Misuse

**Scenario:**
Password reset link:
reset?token=abc123  

👉 Reused or modified token  

**Outcome:**
- Token was not properly invalidated  
- Allowed resetting password multiple times  

**Learning:**
- Tokens must be:
  - Unique  
  - Time-limited  
  - Bound to a specific user/session  

---

### 🧾 4. Accessing Admin API via Header Manipulation

**Scenario:**
Normal request blocked for admin endpoint  

👉 Added header:
X-Role: admin  

**Outcome:**
- Server trusted the header  
- Granted admin-level access  

**Learning:**
- Never rely on client-controlled headers for authorization  

---

### 📂 5. Forced Browsing Sensitive Files

**Scenario:**
Tried accessing hidden endpoints:
/admin  
/reports  
/config  

**Outcome:**
- Some endpoints were accessible without authentication  
- Exposed sensitive configuration data  

**Advanced Observation:**
- Directory enumeration revealed backup files (.bak, .old)

---

### 🔄 6. Privilege Escalation via Role Change

**Scenario:**
User role stored in request:
role=user  

👉 Modified to:
role=admin  

**Outcome:**
- Application granted higher privileges  
- No backend validation  

**Learning:**
- Role validation must always happen server-side  

---

### 📊 7. Multi-Step Workflow Bypass

**Scenario:**
Application required multiple steps for approval:
1. Submit request  
2. Admin approval  
3. Final access  

👉 Directly accessed step 3 endpoint  

**Outcome:**
- Skipped entire approval process  

**Learning:**
- Each step must validate authorization independently  

---

### 🌐 8. API Endpoint Exposure

**Scenario:**
Mobile app API:
GET /api/v1/users  

👉 Tested without authentication  

**Outcome:**
- API returned all user data  

**Advanced Observation:**
- APIs often have weaker access control than web interfaces  

---

## 🧠 Key Insight from These Scenarios

From these practical scenarios, I learned that:

- Broken Access Control is often due to **logic flaws, not technical complexity**  
- Many applications trust **client-side input too much**  
- Testing APIs is just as important as testing web interfaces  
- Thinking in terms of **“what should NOT be allowed”** helps identify vulnerabilities faster  
