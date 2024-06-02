# Config ESLint Guide


## Introduction

This repository which includes configs for ESLint

The following configs are available, and are designed to be used together.

- [Prettier](#prettier)
- [ESLint](#eslint)
- [TypeScript](#typescript)


## Installation

All of our configs are contained in one package. To install:

```sh
# If you use npm
npm i --save-dev @





## ESLint

> Note: ESLint is a peer-dependency of this package, and should be installed
> at the root of your project.
>
> See: https://eslint.org/docs/user-guide/getting-started#installation-and-usage

This ESLint config is designed to be composable.

The following base configs are available. You can use one or both of these
configs, but they should always be first in `extends`:



Note that you can scope configs, so that configs only target specific files.


The following additional configs are available:



> You'll need to use `require.resolve` to provide ESLint with absolute paths,
> due to an issue around ESLint config resolution (see
> [eslint/eslint#9188](https://github.com/eslint/eslint/issues/9188)).

For example, use the shared ESLint config(s) in a Next.js project, set the
following in `.eslintrc.js`.

```js
module.exports = {
  extends: [
    require.resolve('/eslint/browser'),
    require.resolve('/eslint/react'),
    require.resolve('/eslint/next'),
  ],
};
```

### Configuring ESLint for TypeScript

Some of the rules enabled in the TypeScript config require additional type
information, you'll need to provide the path to your `tsconfig.json`.

For more information, see: https://typescript-eslint.io/docs/linting/type-linting

```js
const { resolve } = require('node:path');

const project = resolve(__dirname, 'tsconfig.json');

module.exports = {
  root: true,
  extends: [
    require.resolve('/eslint/node'),
    require.resolve('/eslint/typescript'),
  ],
  parserOptions: {
    project,
  },
  settings: {
    'import/resolver': {
      typescript: {
        project,
      },
    },
  },
};
```

### Configuring custom components for `jsx-a11y`

It's common practice for React apps to have shared components like `Button`,
which wrap native elements. You can pass this information along to `jsx-a11y`
via the `components` setting.

The below list is not exhaustive.



### Scoped configuration with `overrides`

ESLint configs can be scoped to include/exclude specific paths. This ensures
that rules don't "leak" into places where those rules don't apply.

In this example, Jest rules are only being applied to files matching Jest's
default test match pattern.

```js
module.exports = {
  extends: [require.resolve('/eslint/node')],
  overrides: [
    {
      files: ['**/__tests__/**/*.[jt]s?(x)', '**/?(*.)+(spec|test).[jt]s?(x)'],
      extends: [require.resolve('/eslint/jest')],
    },
  ],
};
```

#### A note on file extensions

By default, all TypeScript rules are scoped to files ending with `.ts` and
`.tsx`.

However, when using overrides, file extensions must be included or ESLint will
only include `.js` files.

```js
module.exports = {
  overrides: [
    { files: [`directory/**/*.[jt]s?(x)`], rules: { 'my-rule': 'off' } },
  ],
};
```
