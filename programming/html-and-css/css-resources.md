# CSS resources

[https://htmlcheatsheet.com/css/](https://htmlcheatsheet.com/css/)  
  
`*` _all elements_  
`div` _all div tags_  
`div,p` _all divs and paragraphs_  
`div p` _paragraphs inside divs_  
`div > p` _all p tags, one level deep in div_  
`div + p` _p tags immediately after div_  
`div ~ p` _p tags preceded by div_  
`.classname` _all elements with class_  
`#idname` _element with ID_  
`div.classname` _divs with certain classname_  
`div#idname` _div with certain ID_

`a:link` _link in normal state_  
`a:active` _link in clicked state_  
`a:hover` _link with mouse over it_  
`a:visited` _visited link_  
`p::after{content:"yo";}` _add content after p_  
`p::before` _add content before p_  
`input:checked` _checked inputs_  
`input:disabled` _disabled inputs_  
`input:enabled` _enabled inputs_  
`input:focus` _input has focus_  
`input:in-range` _value in range  
etc..._  
`.div:empty` _element with no children_  
`p::first-letter` _first letter in p_  
`p::first-line` _first line in p_  
`p:first-of-type` _first of some type_  
`p:last-of-type` _last of some type_

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

_Source:_ [_https://css-tricks.com/the-difference-between-nth-child-and-nth-of-type/_](https://css-tricks.com/the-difference-between-nth-child-and-nth-of-type/)\_\_
{% endhint %}

  
__`p:lang(en)` _p with en language attribute_  
`:not(span)` _element that's not a span_  
`p:first-child` _first child of its parent_  
`p:last-child` _last child of its parent_  
`p:nth-child(2)` _second child of its parent_   
`p:nth-child(3n+1)` _nth-child \(an + b\) formula_  
`p:nth-last-child(2)` _second child from behind_  
`p:nth-of-type(2)` _second p of its parent_  
`p:nth-last-of-type(2)` _...from behind_   
`p:only-of-type` _unique of its parent_  
`p:only-child` _only child of its parent_  
`:root` _document's root element_  
`::selection` _portion selected by user_  
`:target`_highlight active anchor_

`a[target]`_links with a target attribute_  
`a[target="_blank"]` _links which open in new tab_  
`[title~="chair"]` _title element containing a word_  
`[class^="chair"]` _class starts with chair_  
`[class|="chair"]` _class starts with the chair word_  
`[class*="chair"]` _class contains chair_  
`[class$="chair"]` _class ends with chair_  
`input[type="button"]` _specified input type_

\_\_

