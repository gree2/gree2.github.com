---
layout: post
title: "python convert notebook to slide using nbconvert"
description: ""
category: [python]
tags: [notebook, slide, nbconvert, serve]
---
{% include JB/setup %}

### ingredients

1. ipython

1. nbconvert

1. reveal.js

### step 1 write you ipython notebook

1. open a new notebook

1. `ipython toolbar` change `cell toolbar`

	1. -

			do nothing cell
			just render it

	1. Slide

			navigate

			left arrow
			right arrow

	1. Sub-Slide

			navigate

			down arrow
			up arrow

	1. Fragment

	1. Skip

	1. Notes

			press 's'

### step 2 install reveal.js

check my post [2015-05-09-getting-revealjs-running-on-your-computer]({% post_url 2015-05-09-getting-revealjs-running-on-your-computer %})

### step 3 convert notebook to slide

1. convert

	* --to

			you can convert notebook to other format
			html
			latex
			pdf
			slides    <= we use this here
			markdown
			rst
			script
			notebook  => open the notebook
			             execute it
			             capture new output
			             save it as `your.nbconvert.ipynb`

			$ ipython nbconvert your.ipynb --to slides --post serve

1. if you don't povide `--post` option, serve it

	* i don't like this, it's not work for me now

			$ python -m SimpleHTTPServer 8000

### step 4 play with your slides

have fun~