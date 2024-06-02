# Config ESLint Guide


## Introduction

This repository which includes configs for ESLint

The following configs are available, and are designed to be used together.

## 🚀 Quick Start

## ✔️ Installation

All of our configs are contained in one package. To install:

```sh
# If you use npm
npm i --save-dev @eslint-config-uit
```



```js
module.exports = {
  extends: [
    require.resolve('@eslint-config-uit/eslint/browser'),
    require.resolve('@eslint-config-uit/eslint/react'),
    require.resolve('@eslint-config-uit/eslint/next'),
  ],
};
```

### ✔️ Configuring ESLint for TypeScript

Some of the rules enabled in the TypeScript config require additional type
information, you'll need to provide the path to your `tsconfig.json`.

For more information, see: https://typescript-eslint.io/docs/linting/type-linting

```js
const { resolve } = require('node:path');

const project = resolve(__dirname, 'tsconfig.json');

module.exports = {
  root: true,
  extends: [
    require.resolve('@eslint-config-uit/eslint/node'),
    require.resolve('@eslint-config-uit/eslint/typescript'),
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



### Scoped configuration with `overrides`

ESLint configs can be scoped to include/exclude specific paths. This ensures
that rules don't "leak" into places where those rules don't apply.

In this example, Jest rules are only being applied to files matching Jest's
default test match pattern.

```js
module.exports = {
  extends: [require.resolve('@eslint-config-uit/eslint/node')],
  overrides: [
    {
      files: ['**/__tests__/**/*.[jt]s?(x)', '**/?(*.)+(spec|test).[jt]s?(x)'],
      extends: [require.resolve('@eslint-config-uit/eslint/jest')],
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
