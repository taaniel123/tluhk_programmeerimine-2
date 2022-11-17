# ESLint

ESlint on tööriist, mis aitab kirjutatud koodi korrastada, hoida koodil ühtlast stiili ja aitab vältida vigu.

[ESlint-i dokumentatsioon.](https://eslint.org/docs/latest/user-guide/getting-started)

James Q Quick video:

[![Watch the video](https://img.youtube.com/vi/KCHg9f2B1I8/maxresdefault.jpg)](https://www.youtube.com/watch?v=KCHg9f2B1I8)

## Typescriptiga projekti korral võiks ESlint-i installeerimine ja seadistamine käia umbes nii:

-   Käsurealt: npm install eslint --save-dev
-   Käsurealt: npx eslint --init
-   Vasta küsimustele:

**How would you like to use ESLint?**

To check syntax, find problems, and enforce code style

**What type of modules does your project use?**

Javascript modules (import/export)

**Which framework does your project use?**

None of these

**Does your project use TypeScript?**

Yes

**Where does your code run?**

Node

**How would you like to define a style for your project?**

Use a popular style guide

**Which style guide do you want to follow?**

Airbnb

**What format do you want your config file to be in?**

JavaScript

**Checking peerDependencies of eslint-config-airbnb-base@latest
The config that you've selected requires the following dependencies:**

**Would you like to install them now with npm?**

Yes

Tõenäoliselt tuleb peale seda vigu moodulite laiendite kohta ja selle kohta, et moodulite asukohta ei saa kätte:
https://newbedev.com/using-eslint-with-typescript-unable-to-resolve-path-to-module
https://www.codegrepper.com/code-examples/typescript/eslint+missing+file+extension+ts

Lõpuks, peale paigaldamist ja seadistamist ja muutmist on minu projektis selline .eslintrc.js fail:

```
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: [
    'airbnb-base',
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 12,
    sourceType: 'module',
  },
  plugins: [
    '@typescript-eslint',
  ],
  rules: {
    'import/extensions': [
      'error',
      'ignorePackages',
      {
        js: 'never',
        jsx: 'never',
        ts: 'never',
        tsx: 'never',
      },
    ],
  },
  settings: {
    'import/resolver': {
      node: {
        extensions: ['.js', '.jsx', '.ts', '.tsx'],
      },
    },
  },
};
```
