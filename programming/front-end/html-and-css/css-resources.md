# CSS resources

{% embed url="https://htmlcheatsheet.com/css/" %}

{% embed url="https://css-tricks.com/snippets/css/a-guide-to-flexbox/" %}



\
\
`*` _all elements_\
__`div` _all div tags_\
__`div,p` _all divs and paragraphs_\
__`div p` _paragraphs inside divs_\
__`div > p` _all p tags, one level deep in div_\
__`div + p` _p tags immediately after div_\
__`div ~ p` _p tags preceded by div_\
__`.classname` _all elements with class_\
__`#idname` _element with ID_\
__`div.classname` _divs with certain classname_\
__`div#idname` _div with certain ID_

`a:link` _link in normal state_\
__`a:active` _link in clicked state_\
__`a:hover` _link with mouse over it_\
__`a:visited` _visited link_\
__`p::after{content:"yo";}` _add content after p_\
__`p::before` _add content before p_\
__`input:checked` _checked inputs_\
__`input:disabled` _disabled inputs_\
__`input:enabled` _enabled inputs_\
__`input:focus` _input has focus_\
__`input:in-range` _value in range_\
_etc..._\
__`.div:empty` _element with no children_\
__`p::first-letter` _first letter in p_\
__`p::first-line` _first line in p_\
__`p:first-of-type` _first of some type_\
__`p:last-of-type` _last of some type_

{% hint style="info" %}
`:first-of-type` is very similar to `:nth-child` but with one **critical difference:** it is [less specific](https://css-tricks.com/the-difference-between-nth-child-and-nth-of-type/).
{% endhint %}

{% hint style="info" %}
Our **:nth-child** selector, in "Plain English," means select an element _if_:

1. It is a paragraph element
2. It is the second child of a parent

Our **:nth-of-type** selector, in "Plain English," means:

1. Select the second paragraph child of a parent

:nth-of-type is... what's a good way to say it... _less conditional_.

_Source:_ [_https://css-tricks.com/the-difference-between-nth-child-and-nth-of-type/_](https://css-tricks.com/the-difference-between-nth-child-and-nth-of-type/)__
{% endhint %}

__\
__`p:lang(en)` _p with en language attribute_\
__`:not(span)` _element that's not a span_\
__`p:first-child` _first child of its parent_\
__`p:last-child` _last child of its parent_\
__`p:nth-child(2)` _second child of its parent_ \
__`p:nth-child(3n+1)` _nth-child (an + b) formula_\
__`p:nth-last-child(2)` _second child from behind_\
__`p:nth-of-type(2)` _second p of its parent_\
__`p:nth-last-of-type(2)` _...from behind_ \
__`p:only-of-type` _unique of its parent_\
__`p:only-child` _only child of its parent_\
__`:root` _document's root element_\
__`::selection` _portion selected by user_\
__`:target`_highlight active anchor_

`a[target]`_links with a target attribute_\
__`a[target="_blank"]` _links which open in new tab_\
__`[title~="chair"]` _title element containing a word_\
__`[class^="chair"]` _class starts with chair_\
__`[class|="chair"]` _class starts with the chair word_\
__`[class*="chair"]` _class contains chair_\
__`[class$="chair"]` _class ends with chair_\
__`input[type="button"]` _specified input type_

__
