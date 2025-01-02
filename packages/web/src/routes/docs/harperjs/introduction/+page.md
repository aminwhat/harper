---
title: Introduction to harper.js
---

## The Mission

If you're a developer, odds are that you are using JavaScript or TypeScript on a daily basis.
Your project probably has a least a little bit of either.

Furthermore, a plurality of focused authorship happens inside either a web browser or an [Electron-based app](https://www.electronjs.org/).
Given this, we wanted to create an environment where trivial to integrate fantastic grammar checking into web applications.
That's why we created `harper.js`.

Today, it serves as the foundation for our [Obsidian plugin](/docs/integrations/obsidian) and our [website](/).

## Installation

`harper.js` is an ECMAScript module designed to be easy to import into any project.
On the inside, it uses a copy of Harper's core algorithm compiled to [WebAssembly](https://webassembly.org/).

It can be imported [natively in a browser](./CDN) or through [npm](https://www.npmjs.com/package/harper.js).

@install-pkg(harper.js)


## Sample Usage:
```js
let harper = require("harper.js");

async function main() {
  // We cannot use `WorkerLinter` on Node.js since it relies on web-specific APIs.
  let linter = new harper.LocalLinter();
  linter.setup();

  let lints = await linter.lint("This is a example of how to use `harper.js`.");

  console.log("Here are the results of linting the above text:");

  for (let lint of lints) {
    console.log(" - ", lint.span().start, ":", lint.span().end, lint.message());

    if (lint.suggestion_count() != 0) {
      console.log("Suggestions:");

      for (let sug of lint.suggestions()) {
        console.log(
          "\t - ",
          sug.kind() == 1 ? "Remove" : "Replace with",
          sug.get_replacement_text(),
        );
      }
    }
  }
}

main();
```
