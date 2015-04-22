---
title: Setting up Jekyll with MathJax
---

Not much to do, basically all you need is to load MathJax from one of
your layouts, for instance:


``` html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8"/>
	<link rel="stylesheet" type="text/css" href="/css/note.css" >
	<script type="text/javascript"
			src="//cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" >
	</script>
  </head>
  <body>
	<h1>{{ page.title }}</h1>
	{{ content }}
  </body>
  </html>
```


Then you will directly be able to include latex commands between `$$`
(this is due to kramdown apparently).

Sweet !
