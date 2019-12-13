# EDM templates

## Tips

\(Assuming you are supporting up to Microsoft Office 2007\)

MSO 2007, 2000 are using Microsoft Word to render emails, only display the first frame of gifs.

1. Easiest way is to start off with email templates \(like Cerberus, linked below\) 
2. Always set doctype, and have the css overrides
3. Use tables for layout, and avoid divs \(does not render on MSO\)
4. Avoid deeply nested tables \(keep file size small\)
5. Use older css \(check out below link to see what css is supported\)
6. Use inline css \(most clients strip the head css out\)
7. Avoid using padding and margins \(if needed, use padding on td tags\)
8. Be aware of font changes and support on mobile devices \(eg: comic sans is not available on mobile\)
9. Use basic/ mso-supported styling tags like bgcolor, valign, background-image, etc
10. Test on litmus across multiple devices \(depends on your users. check with your email service, like sendgrid and mailchimp for a breakdown\)
11. Test on physical devices \(Litmus provides a still image render, which does not guarantee that other things outside of layout are working. Make sure all your links are correct, etc\)
12. Check if your clients cut off your email \(gmail's max size is 102kb\)
13. Check that your utm source in all your links are correct.
14. Check if the backend returns all the needed information.

{% embed url="https://tedgoas.github.io/Cerberus/" %}

{% embed url="https://templates.mailchimp.com/resources/email-client-css-support/" %}

{% embed url="https://sendgrid.com/docs/ui/sending-email/cross-platform-html-design/" %}

{% embed url="https://sendgrid.com/blog/how-to-optimize-your-email-for-mobile/" %}

## Other quirks of EDMs

* Avoid layouts that needs media queries. \(Clients, such as gmail, strips out css in the head, and forwarded emails will break.\)
* You can create conditional statements with fallbacks like this: 

```text
<!--[if mso]>
    <v:shape>...</v:shape>
    <div style="width:0px; height:0px; overflow:hidden; display:none; visibility:hidden; mso-hide:all;">
<![endif]-->

    [fallback goes here]

<!--[if mso]></div><![endif]-->
```

* You need to set your table width for most tables. Do width="600" **without any units** for support in MSO.
* MSO supports vml \(vector-markup-language\), which you will need to create circular images \(as there is no border-radius in MSO\), rounded buttons, etc.
* Unfortunately, these conditional statements are stripped out on forwarding, so do check how it looks on forwarding. Best practice is to use conditional statements sparingly.
* MSO 2007 does not support &lt;a&gt; tags wrapped outside unexpected tags, like a table.
* Lists \(`<ol>` or `<ul>`\) render differently based on client. The spacing might be off, and bullets might be wrong \(eg: Thunderbird renders ul bullets as 0.\)
* There is a default height and width to a table cell. Specifying height/ width on the `<td>` does not work if it's smaller than the default height \(on MSO\). You will need to employ some other ways, like `font-size:1px;` and `mso-table-lspace:0pt; mso-table-rspace:0pt;` accordingly.
* Put `nbsp;` inside your spacer cell. MSO may not render empty cells.



