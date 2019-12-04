# EDM templates

## TLDR

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
12. Check that your utm source in all your links are correct.
13. Check if the backend returns all the needed information.

{% embed url="https://tedgoas.github.io/Cerberus/" %}

{% embed url="https://templates.mailchimp.com/resources/email-client-css-support/" %}

{% embed url="https://sendgrid.com/docs/ui/sending-email/cross-platform-html-design/" %}

{% embed url="https://sendgrid.com/blog/how-to-optimize-your-email-for-mobile/" %}



