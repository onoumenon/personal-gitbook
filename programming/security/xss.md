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

#### Have a whitelist mental model:

Only allow untrusted input in limited places & you should ****use the escape syntax for the part of the HTML document you're putting untrusted data into.

#### Filter input, escape output:

If doing the above, use **tested** libraries and conventions like filter input, escape output. \(See also: [input vs output sanitization](https://security.stackexchange.com/questions/95325/input-sanitization-vs-output-sanitization)\). 

{% hint style="info" %}
Sanitization is a misleading term. It evokes having a blacklist of dangerous tags to be removed, but can also mean other methods. Read [this](https://kevinsmith.io/sanitize-your-inputs).
{% endhint %}

> Escape user-supplied data as late as possible. This ensures that nothing is erroneously assumed to have already been escaped, allowing it to slip through the cracks, _and_ that the appropriate escape method is performed for the given context. For a web application serving up a web page, this would probably be in the HTML template itself since the context is obvious by that point.

{% embed url="http://shiflett.org/blog/2005/filter-input-escape-output" %}



{% hint style="warning" %}
For javascript scripts, the only safe place to put untrusted data is inside a quoted "data value". But there are JS functions that should never take in untrusted input even if escaped, such as [window.setInterval](https://nemethgergely.com/building-secure-javascript-applications/), setTimeout, etc.

Same for some css contexts: 

```text
// Don't allow this
{ background-url : "javascript:alert(1)"; }  // and all other URLs
{ text-size: "expression(alert('XSS'))"; }   // only in IE
```
{% endhint %}



**Use appropriate response headers:** 

To prevent XSS in HTTP responses that aren't intended to contain any HTML or JavaScript, you can use the Content-Type and X-Content-Type-Options headers to ensure that browsers interpret the responses in the way you intend.



{% embed url="https://portswigger.net/web-security/cross-site-scripting" %}

{% embed url="https://cheatsheetseries.owasp.org/cheatsheets/Cross\_Site\_Scripting\_Prevention\_Cheat\_Sheet.html" %}



