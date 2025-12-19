# Web Application Security Testing

## 1. Environment Setup: WebGoat
*WebGoat is a deliberately insecure application maintained by OWASP. It serves as a safe "Sandbox" to practice attacks without breaking real laws.*

**Startup Procedure:**
1.  Ensure Java is installed (`java -version`).
2.  Launch the server from your terminal:
```bash

    java -jar webgoat-server-version.jar
```
3.  Access the interface via your browser:
    * URL: `http://localhost:8080/WebGoat`
    * *Note:* Never expose this interface to the public internet.

---

## 2. Cross-Site Scripting (XSS)
*Concept: Injecting malicious scripts into trusted websites. If the website accepts your text (input) and displays it to other users without cleaning it, the browser executes it as code.*

### Phase A: Proof of Concept (The "Alert" Test)
The goal is to prove that the application is interpreting your input as code rather than just text.

**Payload 1: The Standard Popup**
```html

<script>alert('Vulnerability Found');</script>
```
 * **Why we use it**: *It creates an immediate, undeniable visual confirmation. If the popup appears, the vulnerability exists.* *
 
 **Payload 2: The Stealth Check**
```html

 <script>console.log('XSS Test Log');</script>
```
 * **Why we use it**: *In some environments, popups are blocked or too intrusive. This payload writes silently to the browser's Developer Console (F12).* *
 
### Phase B: Weaponization (Session Hijacking)
*Warning: This demonstrates the actual risk—account takeover.*

**The Attack Logic:** If an attacker can execute JavaScript, they can access sensitive browser data, including the Session Cookie (the temporary ID that keeps a user logged in).

**Payload 3: Cookie Extraction**

```HTML

<script>
    alert("Session Data: " + document.cookie);
</script>
```
* **Note:** *If an attacker gets this string (the Session ID), they can trick the server into thinking they are the logged-in user (e.g., an Admin) without knowing the password.* *

## 3. Defense & Mitigation
1. Ensure the website rejects characters that don't belong (e.g., a "Name" field shouldn't accept `<` or `>`).

2. Convert special characters into safe HTML entities before displaying them (e.g., `<` becomes `&lt;`).

3. Configure a server setting that tells the browser, *"Do not execute scripts unless they come from this specific list of trusted sources."*
