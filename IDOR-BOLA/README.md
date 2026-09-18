# IDOR / BOLA — Can I Still Hack It?

## Lab

**Application:** OWASP Juice Shop
**Challenge:** Can I Still Hack It? — Day 1
**Type:** Web Application Security
**Environment:** Authorized local lab

---

## 🎯 Objective

Test whether an authenticated user can access another user's basket by changing the basket identifier in the request.

---

## 🔎 What I Observed

After signing into my user account and adding an item to my basket, I noticed this API request in Burp Suite:

```http
GET /rest/basket/6
```

The response showed that basket `6` belonged to my user account:

```text
UserID: 24
```

---

## 🧪 What I Tested

I sent the request to Burp Suite Repeater and changed the basket number:

```http
GET /rest/basket/6
```

to:

```http
GET /rest/basket/2
```

The server returned another user's basket.

The basket belonged to:

```text
UserID: 2
```

and contained:

```text
Raspberry Juice (1000ml)
```

---

## 💥 What I Discovered

The application allowed my authenticated account to access another user's basket simply by changing the basket identifier.

This indicated an **authorization problem**.

---

## 🧠 Why It Worked

The application knew that I was authenticated as User 24, but it did not properly verify that I was authorized to access basket 2.

In simple terms:

> **Authentication asks: "Who are you?"**

> **Authorization asks: "Are you allowed to access this?"**

I was authenticated, but the authorization check for the requested basket was insufficient.

---

## 🔐 Vulnerability

**IDOR — Insecure Direct Object Reference**

This behavior also fits the broader concept of **BOLA — Broken Object Level Authorization**, where an application fails to properly enforce authorization when a user requests an object belonging to another user.

---

## 🛠️ Tools Used

* OWASP Juice Shop
* Burp Suite
* Burp Repeater
* Web browser

---

## 📚 What I Learned

* API endpoints can expose object identifiers.
* Changing an identifier can reveal authorization weaknesses.
* Being logged in does not automatically mean you are authorized to access every object.
* IDOR/BOLA is fundamentally about **broken authorization**, not missing authentication.
* I should compare what the application allows me to access with what my account should actually be allowed to access.

---

## 🔁 Can I Still Hack It?

**Result:** ✅ Solved

**Could I identify the issue without being told the vulnerability?**
Yes.

**Could I explain why it worked?**
Yes.

---

## ⚠️ Scope

This testing was performed only against an authorized OWASP Juice Shop laboratory environment for educational purposes.

No real users, websites, or systems were targeted.
