# OWASP Top 10 \(2017\)

### Top 10 order:

1. Injection 
2. Broken Authentication
3. Sensitive data exposure
4. XML External Entities \(XXE\)
5. Broken Access control
6. Security misconfigurations
7. Cross-Site Scripting \(XSS\)
8. Insecure Deserialization
9. Using Components with known vulnerabilities
10. Insufficient logging and monitoring

### Injection

An injection vulnerability occurs when untrusted data is run by the interpreter as part of a command/ query. The most famous is SQL, but can also include NoSQL, OS, and LDAP injections. 

### 

### Broken Authentication

Application functions related to authentication and session management are implemented incorrectly. This leads to compromised passwords/ session tokens/ keys/ accounts.

### 

### Sensitive Data Exposure

Weakly protected sensitive data \(such as ID numbers, financial/ healthcare details\) can lead to identity theft, credit card fraud, etc. Sensitive data requires extra security, such as encryption at rest or in transit, and requires special precautions when exchanged with the browser.



### XML External Entities \(XXE\)

Poorly configured Extensible Markup Language \(**XML**\) processors evaluate external entity references within XML documents. XXE allows an attacker to interfere with an application's processing of XML data. It often allows an attacker to view files on the application server filesystem, and to interact with any backend or external systems that the application itself can access.

{% embed url="https://portswigger.net/web-security/xxe" %}





