---
layout: post
title: "Insert images to GitHub Pages"
date: 2020-07-26
description: How to insert images to GitHub Pages based on Jekyll
share: true
tags:
 - Web
 - Tech-accumulate
---

To insert imges in a markdown file, we should build a new directory save images that should be inserted to your blog articales (e.g. /assets/images/). <span style="color:red;">An absolute path is required</span>. Note if a relative path is being used, the image can not be found and displayed in your webpage.

At first time, we can insert a line in *_config.yml*
```markdown
baseurl: 'your project name'
```

When inserting a image, we can use the variable *baseurl* defined below in an absolute path:
```markdown
![img1]({{site.baseurl}}/assets/images/img1.gif)
```

Last update: 07/26/2020