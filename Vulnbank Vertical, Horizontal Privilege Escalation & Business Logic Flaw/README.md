# Privilege Escalation & Business Logic Testing

## Overview

As part of my **Can I Still Hack It?** practice, I conducted a controlled web-application security simulation using **Burp Suite**.

The exercise focused on understanding how weaknesses in JWT handling, authorization controls, and business logic can affect a banking application.

> **Environment:** Authorized cybersecurity lab
> **Tool:** Burp Suite
> **Focus:** JWT, privilege escalation, authorization, and business logic

## 1. Vertical Privilege Escalation

I inspected the JWT associated with a normal user account and identified an `isAdmin` claim.

The original value was:

```text
is_Admin=false
```

For the purpose of the authorized lab simulation, I changed the claim to:

```text
is_Admin=true
```

I then generated the modified token and tested it within the application's authorization flow.

### Result

The application allowed the user session to access administrator-level functionality.

This demonstrated a **vertical privilege escalation** scenario: moving from a lower-privileged user context to a higher-privileged administrative context.

---

## 2. Horizontal Privilege Escalation

I then investigated authorization between different user accounts.

Using the application's authentication mechanism, I tested tokens associated with different users.

I was able to access information associated with other accounts, including:

* Username
* User ID
* Bank balance

### Result

The application did not adequately enforce the boundary between users.

This demonstrated a **horizontal privilege escalation** scenario, where one user could access another user's information at the same privilege level.

---

## 3. Business Logic Testing

The banking functionality provided another interesting test case.

I examined the application's transfer workflow and tested how authorization was enforced when performing transactions.

Using the appropriate authorization context within the controlled lab, I was able to perform a transfer involving another user's account and move funds into my test account.

I subsequently tested the withdrawal functionality against the resulting balance.

### Result

The workflow demonstrated how weaknesses in authorization combined with business-logic controls can potentially allow actions that should be restricted by the application's rules.

---

## Key Lessons

This exercise reinforced three important concepts:

### Authentication

**Who are you?**

The application identifies the user through an authentication mechanism such as a JWT.

### Authorization

**What are you allowed to do?**

Being authenticated does not automatically mean a user should be able to access another user's data or administrator functionality.

### Business Logic

**What actions should you be allowed to perform?**

Even when individual endpoints appear to work correctly, weaknesses in the application's workflow can allow users to perform actions that violate intended business rules.

## Takeaways

* JWT claims must be properly validated server-side.
* Privileged operations require server-side authorization.
* Users should only access resources belonging to their authorized scope.
* Sensitive banking operations require strong authorization checks.
* Business workflows should be tested as complete sequences rather than only testing individual endpoints.
* Burp Suite provides useful visibility into how authentication and authorization are implemented at the HTTP level.

## Conclusion

This lab helped me connect three areas that are easy to study separately:

**JWT → Authorization → Business Logic**

The important part of the exercise wasn't simply changing a value in a token. It was understanding **why the application trusted that value and what happened when authorization controls failed.**

**Authorized lab simulation only.**

#CanIHackIt #CyberSecurity #Pentesting #PortSwigger #BurpSuite #WebSecurity #EthicalHacking #HomeLab
