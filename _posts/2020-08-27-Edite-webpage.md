---
layout: post
title: "A demand for editing a webpage"
date: 2020-08-27
description: How to edite a GitHub blog webpage
share: true
tags:
 - Web
 - Tech-accumulate
---

In my default template of the GitHub blog, the title of a post weabpage is set to be a hyperlink. Now I don't need the title to be a hyperlink but a pure text, how should I do?

First, we need to find where to modify the code. This is associated with layouts, thus we enter directory *_layouts* and find a htmml file *post.html*. Yes, here is where to modify the code! In *post.html* we find a line of code as follows.

```html
<h1><a href="{{ page.url | relative_url }}">{{ page.title }}</a></h1>
```
<a>.....</a> denotes the hyperlink to page tile. The solution is simple since we just need to delete <a>.....</a> to make it a pure text. We can also designate the color of the text.

```html
<h1 style="color: rgb(7, 119, 224)">{{ page.title }}</h1>
```

Last update: 08/27/2020
