# Vue Code Highlighter
<p align="left">
<a href="https://www.npmjs.com/package/vue-code-highlighter"><img
          src="https://img.shields.io/bundlephobia/minzip/vue-code-highlighter" /> </a>
      <a class="mx-2" href="https://www.npmjs.com/package/vue-code-highlighter"><img
          src="https://img.shields.io/npm/dt/vue-code-highlighter.svg" alt="NPM Downloads"></a>
      <a href="http://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-blue.svg"
          alt="License"></a>
  
</p>

Vue component for highlighting code or syntax with modern style, copy functionality, multicode and powered by Highlight.js
<p align="center" width="100%">
    <img width="80%" src="https://i.ibb.co/2MgrHV0/image.png"> 
</p>

## Installation

```bash
npm install vue-code-highlighter highlight.js

// or use yarn
yarn add vue-code-highlighter highlight.js
```

## Usage

### Importing

This package support either composition and options api with typescript or javascript.

```ts
import { VueCodeHighlighter, VueCodeHighlighterMulti } from 'vue-code-highlighter';
import 'vue-code-highlighter/dist/style.css'
```

This package does not need to call CSS from the highlight.js package because I have already wrapped it. What you need to do is install the highlight.js package.
### Example & Demo

Example code and demo available in <a href="https://ichsanputr.github.io/vue-code-highlighter">ichsanputr.github.io/vue-code-highlighter</a>


## Props

There are 2 types of components, namely VueCodeHighlighter and VueCodeHighlighterMulti, both of them have different props.

<b>VueCodeHighlighter</b>

| Attribute    |   Type   |    Is Required     | Description                                                                   |
|:-------------|:--------:|:--------------:|:------------------------------------------------------------------------------|
| lang      |  String  |       true       | Code languange from Highlighter cheatset, see <a href="https://github.com/highlightjs/highlight.js/blob/main/SUPPORTED_LANGUAGES.md">Supported languange</a>                                              |
| code         |  String  |   true    | Plain code which will be displayed,  please wrap your code using backtick (``)           |
| title     |  String  | no | Custom title in header |
| disableCopy     |  Boolean  |     no     | Disable copy button and copy functionality to code        |

<b>VueCodeHighlighterMulti</b>

| Attribute    |   Type   |    Is Required     | Description                                                                   |
|:-------------|:--------:|:--------------:|:------------------------------------------------------------------------------|
| code      |  Array  |       true       | See example below                                         |
 disableCopy     |  Boolean  |     no     | Disable copy button and copy functionality to code        |

<p>Example for lang props in Multi Code.</p>

```js
const codeMulti = [
  {
    lang: "js",
    title: "vue",
    code: codeVue.value
  },
  {
    lang: "html",
    code: codeHtml.value,
    title: 'html'
  },
  {
    lang: "css",
    code: codeCss.value,
  },
]
```
