---
layout: default
title:  "deno"
description: Deno jupyter kernel snippets
---

# Deno jupyter kernel

Render a React component

```js
/** @jsxImportSource https://esm.sh/react@18 */

import { document } from "jsr:@ry/jupyter-helper";
import { renderToStaticMarkup } from "https://esm.sh/react-dom@18/server";

function Greeting({ name }) {
  return (
    <div style={{ fontFamily: "sans-serif", color: "steelblue" }}>
      <h2>Hello, {name}!</h2>
      <p>This is a React component rendered inside Jupyter ✨</p>
    </div>
  );
}

const container = document.createElement("div");
container.innerHTML = renderToStaticMarkup(<Greeting name="Edward" />);

export default container;
```

Render JSX

```js
/** @jsxImportSource https://esm.sh/react@18 */

import { document } from "jsr:@ry/jupyter-helper";
import { renderToStaticMarkup } from "https://esm.sh/react-dom@18/server";

const container = document.createElement("div");
container.innerHTML = renderToStaticMarkup(<h1>Hello, World!</h1>);

export default container;
```


