# ts-composite-error

This repository illstrates a problem that I've run into caused by the intersection of
`composite` TypeScript projects with `allowJs: false` and using `@ts-expect-error`
to allow importing a `.js` file...

In this setup we have three packages:

* `package-a` - imports a class from a `.js` file from `package-b` with
  `@ts-expect-error` and exports a subclass of it
* `package-b` - provides that `.js` file for import
* `package-c` - imports the subclass exported from `package-a`

When you try to build this app you get an error:

> packages/package-a/declarations/src/index.d.ts:1:21 - error TS2307: Cannot find module 'ts-composite-error-b/foo' or its corresponding type declarations.
>
> 1 import { Foo } from 'ts-composite-error-b/foo';

Note that the error is present because the `.d.ts` is being type checked and
it _doesn't_ contain the `@ts-expect-error` comment.

`package-a` works fine by itself but not when consumed from _another_
package