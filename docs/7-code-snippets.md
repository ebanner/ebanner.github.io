---
layout: default
title: Code Snippets
permalink: /code-snippets/
---

# Code Snippets

Use JSX in a Deno jupyter kernel

```js
/** @jsxImportSource https://esm.sh/react@18 */

import { document } from "jsr:@ry/jupyter-helper";
import { renderToStaticMarkup } from "https://esm.sh/react-dom@18/server";

const container = document.createElement("div");
container.innerHTML = renderToStaticMarkup(<h1>Hello, World!</h1>);
export default container;
```

<h3 style="margin-bottom: 0;">
  <img
    src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png"
    width="18"
    style="vertical-align:-3px; margin-right:4px;"
  >
  <a href="https://github.com/ebanner/dotfiles/blob/master/ipython/profile_default/startup/00-custom-magics.py">
    <code>%image</code> magic command
  </a>
</h3>
<p style="margin-top: 5px; color: #666; font-size: 16px;">
  IPython <a href="https://ipython.readthedocs.io/en/stable/interactive/magics.html">
    magic command 
  </a>
  for displaying an image in a jupyter notebook
</p>
