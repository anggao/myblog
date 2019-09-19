---
title: "jekyll configuration"
date: "2013-08-28"
template: "post"
draft: false
slug: "/posts/jekyll-configuration/"
category: "jekyll"
tags:
  - "jekyll"
description: ""
socialImage: ""
---

### insert pictures in the post

+ add variable `img_url: http://anggao.github.io/assets/images` in `_config.yml`
+ store pictures in `assets/images`
+ refer a picture in the post by `![ruby]({{site.img_url}}/ruby.png})`

![ruby]({{site.img_url}}/ruby.gif)

### support Latex

+ add following code block in `_layouts/default.html`

```javascript
<!-- MathJax Section -->
 
    <script type="text/javascript"
    src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
    <script>
        MathJax.Hub.Config({
              tex2jax: {
              skipTags: ['script', 'noscript', 'style', 'textarea', 'pre']
              }
        });
        MathJax.Hub.Queue(function() {
            var all = MathJax.Hub.getAllJax(), i;
            for(i=0; i < all.length; i += 1) {
                all[i].SourceElement().parentNode.className += ' has-jax';
            }
        });
    </script>
```

+ modify style.css

```css
body div.content {}
    body div.content code.has-jax {
        font: inherit;
        font-size: 100%;
        background: inherit;
        border: inherit;
        color: #000000;
    }
```

This is an inline integral: `\(\int_a^bf(x)dx\)`
