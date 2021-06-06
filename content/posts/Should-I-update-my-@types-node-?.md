---
title: Should I update my `@types/node` ?
date: 2021-06-06T14:53:41+01:00
lastmod: 2021-06-06T14:53:41+01:00
author:
  - C-A de Salaberry
# authorlink: https://author.site
cover: /img/cover.jpg
categories:
  - javascript
tags:
  - package
  - javascript
  - node
draft: false
---
Should I update my `@types/node` ?

TLDR: use a tilde (`~`) in your `packages.json` to have the most accurate version

When updating my libraries, I was always wondering what the relation between node and `@types/node` was.
After sone digging, I landed onto the following:

```
// https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/node/index.d.ts
// Type definitions for non-npm package Node.js 15.12
// Project: http://nodejs.org/
// Definitions by: Microsoft TypeScript <https://github.com/Microsoft>
//                 DefinitelyTyped <https://github.com/DefinitelyTyped>
```

The `@types/node` package follows the node version with a twist. The last digit (patch) of the version is used for patches in the `@types/node` package AND patches in the `node` version.

If you want to keep your packages up to date, it is advised to use the latest release for your version of node.

If you are on `node@v14.17.0` you should use the latest `@types/node@v14.17.x` version (`14.17.2` at the time of writing)
