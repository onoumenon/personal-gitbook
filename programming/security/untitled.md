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

Poorly configured Extensible Markup Language \(**XML**\) processors evaluate external entity references within XML documents. It often allows an attacker to view files on the application server filesystem, and to interact with any backend or external systems that the application itself can access.

{% embed url="https://portswigger.net/web-security/xxe" %}



### Broken Access Control

Poorly enforced restrictions on what users can do. Attackers can exploit the flaws to access other users' accounts, view sensitive files, modify other users’ data, change access rights, etc.

{% embed url="https://portswigger.net/web-security/access-control" %}



### Security Misconfig

This results from insecure default configurations, incomplete or ad hoc configurations, open cloud storage, misconfigured HTTP headers, and verbose error messages containing sensitive information. All operating systems, frameworks, libraries, and applications should be securely configured, plus patched and upgraded. 



### XSS

Happens when an application includes untrusted data in a new web page without proper validation or escaping, or updates an existing web page with user-supplied data using a browser API that can create HTML or JavaScript. Leads to hijacked user sessions, and malicious redirects and defaced sites.



### Insecure deserialization

{% hint style="info" %}
* Serialization is the process that converts an [object](https://searchmicroservices.techtarget.com/definition/object) to a format that can later be restored. Deserialization is the opposing process which takes data from a file, stream or network and rebuilds it into an object. [source](https://searchsecurity.techtarget.com/definition/insecure-deserialization)
{% endhint %}

An attacker can inject hostile serialized objects to a [web app](https://searchsoftwarequality.techtarget.com/definition/Web-application-Web-app), where the victim’s computer would initialize deserialization of the hostile data. 



### Using vulnerable components

Third-party libraries and frameworks with known vulnerabilities that compromise the overall application security.



### Insufficient logging and monitoring

Lack of logging/ monitoring exacerbates a breach. Most breach studies show time to detect a breach is over 200 days, typically detected by external parties rather than internal processes or monitoring.



