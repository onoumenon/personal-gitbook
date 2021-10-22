# What to do if you've pushed secrets to github

{% hint style="warning" %}
**Warning: Once you have pushed a commit to GitHub, you should consider any data it contains to be compromised.** If you committed a password, change it! If you committed a key, generate a new one.

This article tells you how to make commits with sensitive data unreachable from any branches or tags in your GitHub repository. However, it's important to note that those commits may still be accessible in any clones or forks of your repository, directly via their SHA-1 hashes in cached views on GitHub, and through any pull requests that reference them. You can't do anything about existing clones or forks of your repository, but you can permanently remove cached views and references to the sensitive data in pull requests on GitHub by contacting [GitHub Support](https://support.github.com/contact) or [GitHub Premium Support](https://premium.githubsupport.com).
{% endhint %}



Follow the instructions here:

{% embed url="https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository" %}

Follow 'Advice specific to a key' if you are using a specific provider

{% embed url="https://github.com/GitGuardian/APISecurityBestPractices/blob/master/Leak%20Mitigation%20Checklist.md#122-detailed-procedure" %}

