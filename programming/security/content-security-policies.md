# Content Security Policies

CSP provides a standard method for website owners to declare approved origins of content that browsers should be allowed to load on that website.

{% embed url="https://developers.google.com/web/fundamentals/security/csp" %}



## TL;DR

* Use whitelists to tell the client what's allowed and what isn't.
* Learn what directives are available. \('default-src', 'script-src', etc\)
* Learn the keywords they take. \('none', self, etc\)
* Inline code and `eval()` are considered harmful.
* Report policy violations to your server before enforcing them.



## Sources whitelist:

{% embed url="https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP" %}

```text
Content-Security-Policy: script-src 'self' https://apis.google.com
```

`script-src`: a fetch directive

then followed by the sources 

Other fetch directives: [https://developer.mozilla.org/en-US/docs/Glossary/Fetch\_directive](https://developer.mozilla.org/en-US/docs/Glossary/Fetch_directive)

{% hint style="info" %}
Make sure that you list _all_ required resources of a specific type in a _single_ directive. If you wrote something like `script-src https://host1.com; script-src https://host2.com` the second directive would simply be ignored.



Rather, it should be written like this:

```text
script-src https://host1.com https://host2.com
```
{% endhint %}



## Implementation details

* Policy is defined on a page-by-page basis. \(need to send http header with every response\)
* The source list in each directive is flexible. \(can be specified by scheme \(`data:`, `https:`\)/ hostname\(`example.com`\)/ full uri\(`example.com`\)
* source-list accepts other keywords: 
  * **`'none'`**, as you might expect, matches nothing.
  * **`'self'`** matches the current origin, but not its subdomains.
  * **`'unsafe-inline'`** allows inline JavaScript and CSS. \(We'll touch on this in more detail in a bit.\)
  * **`'unsafe-eval'`** allows text-to-JavaScript mechanisms like `eval`. \(We'll get to this too.\)

### Inline code is considered harmful <a id="inline_code_is_considered_harmful"></a>

If an attacker can inject a script tag that directly contains some malicious payload \(`<script>sendMyDataToEvilDotCom();</script>`\), the browser has no mechanism by which to distinguish it from a legitimate inline script tag. CSP solves this problem by banning inline script entirely: it's the only way to be sure. That means you may have to refactor inline event handlers.

```text
<script>
  function doAmazingThings() {
    alert('YOU AM AMAZING!');
  }
</script>
<button onclick='doAmazingThings();'>Am I amazing?</button>
```

to something more like:

```text
<!-- amazing.html -->
<script src='amazing.js'></script>
<button id='amazing'>Am I amazing?</button>
```

```text
// amazing.js
function doAmazingThings() {
  alert('YOU AM AMAZING!');
}
document.addEventListener('DOMContentLoaded', function () {
  document.getElementById('amazing')
    .addEventListener('click', doAmazingThings);
});
```

### Eval too

Eval converts  inert text into executable JavaScript and executing it on their behalf. Eg: `eval()`, `new Function()`, `setTimeout([string], ...)`, and `setInterval([string], ...)`

Rewrite any `setTimeout` or `setInterval` calls you're currently making with inline functions rather than strings. For example:

However, a better choice would be a templating language that offers precompilation \([Handlebars does](http://handlebarsjs.com/precompilation.html), for instance\). Precompiling your templates can make the user experience even faster than the fastest runtime implementation, and it's safer too.

## Reporting

You can instruct the browser to `POST` JSON-formatted violation reports to a location specified in a `report-uri` directive.

```text
Content-Security-Policy: default-src 'self'; ...; report-uri /my_amazing_csp_report_parser;
```

Sample report:

```text
{
  "csp-report": {
    "document-uri": "http://example.org/page.html",
    "referrer": "http://evil.example.com/",
    "blocked-uri": "http://evil.example.com/evil.js",
    "violated-directive": "script-src 'self' https://apis.google.com",
    "original-policy": "script-src 'self' https://apis.google.com; report-uri http://example.org/my_amazing_csp_report_parser"
  }
}
```

