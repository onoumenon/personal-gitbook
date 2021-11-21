# Authorization

{% embed url="https://research.nccgroup.com/2020/04/21/code-patterns-for-api-authorization-designing-for-security" %}

### TLDR:&#x20;

4 code patterns: ad-hoc, route-based, centralized, object-based

centralized and object-based preferred for most applications

centralized is easier to audit till it gets too large and migrate to object-based



{% embed url="https://kel.bz/post/authz" %}

Key Principle 1: Design an access control model.

Key Principle 2: Don't trust client-provided data when authenticating or authorizing a user.

Key Principle 3: Deny access by default.

Key Principle 4: Be abstract and centralized.

{% embed url="https://casbin.org/docs/en/abac" %}

{% embed url="https://en.wikipedia.org/wiki/Computer_security_model" %}

{% embed url="https://www.ekransystem.com/en/blog/rbac-vs-abac" %}

\


{% embed url="https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html" %}

{% embed url="https://stackoverflow.com/questions/22375880/best-design-pattern-to-control-permissions-on-a-per-object-per-user-basis-with" %}

{% embed url="https://transang.me/pattern-oriented-software-architecture-access-control-pattern" %}

{% embed url="https://epub.uni-regensburg.de/6503/1/Trustbus_114.pdf" %}

{% embed url="https://en.wikipedia.org/wiki/Role-based_access_control" %}

{% embed url="https://medium.com/innoventes/django-protect-apis-using-djangomodelpermissions-cb1bf65c5b58" %}

{% embed url="https://stackoverflow.com/questions/25960850/loading-initial-data-with-django-1-7-and-data-migrations" %}
