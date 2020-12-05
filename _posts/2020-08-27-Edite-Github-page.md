---
layout: post
title: " Editing a Github page"
date: 2020-08-27
description: How to edite a GitHub blog webpage
share: true
tags:
 - Web
---

## Adjust title

In my default template of the GitHub blog, the title of a post weabpage is set to be a hyperlink. Now I don't need the title to be a hyperlink but a pure text, how should I do?

First, we need to find where to modify the code. Since this demand is associated with layouts, thus we enter directory *_layouts* and find a htmml file *post.html*. Yes, here is where to modify the code! In *post.html* we find a line of code as follows.

```html
<h1><a href="{{ page.url | relative_url }}">{{ page.title }}</a></h1>
```
"<a..>...</a>" denotes the hyperlink to page tile. The solution is simple since we just need to delete "<a..>...</a>" to make it a pure text. We can also designate the color of the text.

```html
<h1 style="color: rgb(7, 119, 224)">{{ page.title }}</h1>
```

## Adjust highlight code 

The defual font size of the highlight code is too small. How to make it larger?
Entering `assets\css` and open `syntax.scss`. 
```markdown
.highlight {
  margin-bottom: 1.5em;
  color: $foreground;
  background-color: $background;
  @include rounded(4px);
  pre {
    position: relative;
    margin: 0;
    padding: 1em;
    overflow-x: auto;
    word-wrap: normal;
    border: none;
    white-space: pre;
    background-color: rgb(77, 76, 76);
    color: #ddd !important;
    code {
      white-space: pre;
      color: #ddd;
    }
```

Add two lines to adjust font size: `font-size: .9375em;`

```markdown
.highlight {
  margin-bottom: 1.5em;
  color: $foreground;
  background-color: $background;
  @include rounded(4px);
  pre {
    position: relative;
    margin: 0;
    font-size: .9375em;
    padding: 1em;
    overflow-x: auto;
    word-wrap: normal;
    border: none;
    white-space: pre;
    background-color: rgb(77, 76, 76);
    color: #ddd !important;
    code {
      white-space: pre;
      color: #ddd;
      font-size: .9375em;
    }
```

Reference: <https://github.com/academicpages/academicpages.github.io/issues/59>


##  Automatically add serial number for titles

Add the following code to a new css file `auto-number-title.css`. Then add the following line to each blog file (.md) to be posted. (i.e. Add a reference from an external style sheet to the .md file)

```markdown
<link rel="stylesheet" type="text/css" href="/jekyll-clean-dark/assets/css/auto-number-title.css"/>
```

```html
h1 { counter-reset: h2counter; }
h2 { counter-reset: h3counter; }
h3 { counter-reset: h4counter; }
h4 { counter-reset: h5counter; }
h5 { counter-reset: h6counter; }
h6 { }
h2:before {
  counter-increment: h2counter;
  content: counter(h2counter) ".\0000a0\0000a0";
}
h3:before {
  counter-increment: h3counter;
  content: counter(h2counter) "."
            counter(h3counter) ".\0000a0\0000a0";
}
h4:before {
  counter-increment: h4counter;
  content: counter(h2counter) "."
            counter(h3counter) "."
            counter(h4counter) ".\0000a0\0000a0";
}
h5:before {
  counter-increment: h5counter;
  content: counter(h2counter) "."
            counter(h3counter) "."
            counter(h4counter) "."
            counter(h5counter) ".\0000a0\0000a0";
}
h6:before {
  counter-increment: h6counter;
  content: counter(h2counter) "."
            counter(h3counter) "."
            counter(h4counter) "."
            counter(h5counter) "."
            counter(h6counter) ".\0000a0\0000a0";
}
```

Last update: 08/27/2020

<link rel="stylesheet" type="text/css" href="/jekyll-clean-dark/assets/css/auto-number-title.css"/>