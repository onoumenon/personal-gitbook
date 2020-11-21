# EDM templates

{% hint style="info" %}
This is copied from the documentation I wrote before leaving my first job, where I was responsible for EDM templates. EDM templates are rendered in Mail clients and css/ html support lags behind browsers, so this neccessitated a different workflow. Eg: testing is done in Litmus, which generates snapshots from different clients.
{% endhint %}

#### Context

We have sendgrid email templates that can be accessed via the Heroku’s dashboard.  
Email templates have a different set of guidelines than web development due to the different limitations of email clients from browsers.  
  
If you’re editing the templates, do take note to send test emails.

#### Resources

You can check out the below resources first to get the general guidelines:

* [Mailchimp's Guide](https://templates.mailchimp.com/) \(more comprehensive\)
* [Sendgrid's Guide](https://sendgrid.com/docs/ui/sending-email/cross-platform-html-design/) \(just the tips\)
* [CSS support](https://templates.mailchimp.com/resources/email-client-css-support/)

#### Optimization 

Do optimize if email template is big. You should already use the smallest file sizes possible \(for images, etc\).  
Why? Because clients like Gmail cuts off at 102kb.

* [Mobile Optimization](https://sendgrid.com/blog/how-to-optimize-your-email-for-mobile/)
* [Minification](https://www.emailonacid.com/blog/article/email-development/how-to-minify-email-html/), [HtmlCrush](https://htmlcrush.com/dark/)

## Specific steps/ checklist

Assumes that you are supporting up to Microsoft Office 2007. Caveat: As with browser support, this may have changed. Check the current support/ best practices if this document is older than 2 years.

1. Use your IDE to edit/ create an email template. \(Sendgrid’s editor is notoriously bad\) Use a [Handlebar Preview](https://marketplace.visualstudio.com/items?itemName=greenbyte.handlebars-preview) extension to view the preview with json data. To use the json data, in the same folder, have your hbs and json in the following naming convention:   ├── 📁 Welcome Email: Fairprice Sept2020  └── email.hbs  └── email.hbs.json
2. If maintaining/ editing existing template, please save a copy of the existing one locally. If starting from scratch, start off with established email templates. Our current templates are based on Cerberus \(link [here](https://tedgoas.github.io/Cerberus/)\).
3. Always set the doctype, and have the css resets for a clean slate \(This is already included in the Cerberus templates with detailed comments\).
4. Use tables for layout, and avoid divs, as it's not supported in MS Office. In general, KISS.
5. Avoid deeply nested tables \(this keeps file size small\).
6. Use older css \(check out [this link](https://templates.mailchimp.com/resources/email-client-css-support/) to see what css is supported\).
7. Use inline css \(some clients strip the head css out\).
8. Avoid using padding and margins \(read [here](https://www.emailonacid.com/blog/article/email-development/7_tips_and_tricks_regarding_margins_and_padding_in_html_emails/) on guideline\).
9. Be aware of font changes and [support](https://www.litmus.com/blog/the-ultimate-guide-to-web-fonts/) on mobile devices/ clients. Use websafe fonts, and supported embed methods.
10. Use basic/ mso-supported styling tags like bgcolor, valign, background-image, width, height, etc. This adds support for older MSO clients.
11. There may be a need for [mso-conditional statements](https://stackoverflow.design/email/base/mso/) which only renders on mso clients, for elements mso clients can’t support. Example use case: conditional fallback with circle vml for mso clients for a rounded image, as mso does not support border-radius. Tip: start off with a [bullet-proof element](https://www.emailonacid.com/blog/article/email-development/how-to-make-your-emails-bulletproof/)
12. Test on litmus across multiple devices for new templates/ massive changes \(supported devices depend on your users. check with your email service, sendgrid, for a breakdown of what clients your users use\).
13. Test on physical devices \(Litmus provides a still image render, which does not guarantee that other things outside of the layout are working. Make sure all your links are correct, etc\).
14. Check if your clients cut off your email \(gmail's max size is 102kb\).
15. Check that your utm source in all your links are correct.
16. Check if the backend returns all the needed information.‌

## Other quirks of Email Clients \(specifically MSO\)

If you come across an issue and you’re following best practices, chances are it could be a quirk.  
Non-exhaustive [list of client-specific quirks](http://freshinbox.com/resources/css.php#clientquirks)  
Other ones that might be relevant:

* MSO 2007, 2000 are using Microsoft Word to render emails, and only display the first frame of gifs.
* Avoid layouts that needs media queries. \(Clients, such as gmail, strips out css in the head, and forwarded emails will break.\)
* You will probably need MSO fallbacks, and they look like comments. **DO NOT DELETE THEM**

```text
<!--[if mso]>    <v:shape>...</v:shape>    <div style="width:0px; height:0px; overflow:hidden; display:none; visibility:hidden; mso-hide:all;"><![endif]-->
    [fallback goes here]
<!--[if mso]></div><![endif]-->‌
```

* You need to set your table width for most tables. Do width="600" **without any units** for support in MSO.
* MSO supports vml \(vector-markup-language\), which you will need to create circular images for it \(as there is no border-radius in MSO\), rounded buttons, etc.
* Unfortunately, these conditional statements are stripped out on forwarding, so do check how it looks on forwarding. Best practice is to use conditional statements sparingly, for non-essential styling, etc.
* MSO 2007 does not support &lt;a&gt; tags wrapped outside unexpected tags, like a table. Use vanilla html.
* Lists \(&lt;ol&gt; or &lt;ul&gt;\) render differently based on client. The spacing might be off, and bullets might be wrong \(eg: Thunderbird renders ul bullets as 0.\)
* There is a default height and width to a table cell. Specifying height/ width on the &lt;td&gt; does not work if it's smaller than the default height \(on MSO\). You will need to employ some other ways, like font-size:1px; and mso-table-lspace:0pt; mso-table-rspace:0pt; accordingly.
* Put nbsp; inside your spacer cell. Certain MSO clients may not render empty cells.

