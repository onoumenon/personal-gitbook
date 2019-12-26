# XSS

Cross Site Scripting \(XSS\) is a vulnerability that allows attackers to inject malicious code into contents of a website not under their control, bypassing the browser's [same origin policy](https://www.google.com/search?q=same+origin+policy).

## Types

In a **reflected XSS** attack, the attack is in the **request** itself \(frequently the URL\), and the server inserts the attack in the response \(reflecting the attack code as an error message, eg\) without sanitisation. 

In a **stored XSS** attack, the attacker stores the attack in the **server** \(e.g., in a snippet\) and the victim triggers the attack by browsing to a page on the server that renders the attack, by not properly escaping or sanitizing the stored data.

## Syntax

#### XSS with script in attributes:

```text
<body onload=alert('test1')>
```

**XSS using Script Via Encoded URI Schemes:**

```text
<IMG SRC=j&#X41vascript:alert('test2')>
```

**XSS using code encoding:**

```text
<META HTTP-EQUIV="refresh"
CONTENT="0;url=data:text/html;base64,PHNjcmlwdD5hbGVydCgndGVzdDMnKTwvc2NyaXB0Pg">
```

**Others:** [XSS Filter Evasion Cheat Sheet](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet)

## Preventation

#### Have a whitelist model for input:

Only allow untrusted input in limited places & you **MUST** use the escape syntax for the part of the HTML document you're putting untrusted data into.

Use **tested** html sanitizers like [sanitize-html](https://www.npmjs.com/package/sanitize-html). \(See also: [input vs output sanitization](https://security.stackexchange.com/questions/95325/input-sanitization-vs-output-sanitization)/ encoding\)

For javascript scripts, the only safe place to put untrusted data is inside a quoted "data value".

**Use appropriate response headers.** To prevent XSS in HTTP responses that aren't intended to contain any HTML or JavaScript, you can use the Content-Type and X-Content-Type-Options headers to ensure that browsers interpret the responses in the way you intend.

{% hint style="warning" %}
There are JS functions that should never take in untrusted input even if escaped, such as [window.setInterval](https://nemethgergely.com/building-secure-javascript-applications/), setTimeout, etc.

Same for some css contexts: 

```text
// Don't allow this
{ background-url : "javascript:alert(1)"; }  // and all other URLs
{ text-size: "expression(alert('XSS'))"; }   // only in IE
```
{% endhint %}

{% embed url="https://portswigger.net/web-security/cross-site-scripting" %}

{% embed url="https://cheatsheetseries.owasp.org/cheatsheets/Cross\_Site\_Scripting\_Prevention\_Cheat\_Sheet.html" %}



