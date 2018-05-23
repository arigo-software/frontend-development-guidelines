# Frontend Development Guidelines

This document aims to provide the ground rules for JavaScript code, such that it is highly readable and consistent across Arigo-Software's frontend team.

To achieve this, we are using a couple of tools to help us maintain the code base.

Remember to **respect and maintain the formatting of an existing document**, meaning that your code should fail the review process if it doesn't maintain consistency with the rest of the document.

## EditorConfig

The base tool to assure consistent spacing across every file in the application is an [EditorConfig](http://editorconfig.org) configuration file.

Here is the default `.editorconfig` file:

```sh
# editorconfig.org

root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[.md]
insert_final_newline = false
trim_trailing_whitespace = false
```

## ESLint, stylelint & Prettier

Utilities we are using:
  - [ESLint](https://eslint.org), a JavaScript linting utility
  - [stylelint](https://stylelint.io), CSS linting utility
  - [Prettier](https://prettier.io), an opinionated code formatter
  
Together they analyse the code to find problematic patterns or code that doesn’t adhere to our style guidelines.

If your editor supports it, you will get a feedback while editing your code. Otherwise, your code gets analysed on commit. **If your code does not fit our guidelines, you cannot commit it!**

To achieve this functionality, several Node packages must be installed and configuration files added.

Node packages:

+ [babel-eslint](https://github.com/babel/babel-eslint)
+ [eslint](https://github.com/eslint/eslint)
+ [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)
+ [eslint-plugin-html](https://github.com/BenoitZugmeyer/eslint-plugin-html)
+ [eslint-plugin-import](https://github.com/benmosher/eslint-plugin-import)
+ [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier)
+ [husky](https://github.com/typicode/husky)
+ [lint-staged](https://github.com/okonet/lint-staged)
+ [prettier](https://github.com/prettier/prettier)
+ [stylelint](https://github.com/stylelint/stylelint)
+ [stylelint-config-prettier](https://github.com/shannonmoeller/stylelint-config-prettier)
+ [stylelint-config-recommended](https://github.com/stylelint/stylelint-config-recommended)
+ [stylelint-order](https://github.com/hudochenkov/stylelint-order)

For ESlint add this `.eslintrc.json` file to your root:

```json
{
  "env": {
    "browser": true,
    "es6": true
  },
  "parser": "babel-eslint",
  "extends": [
    "eslint:recommended",
    "plugin:import/errors",
    "plugin:import/warnings",
    "plugin:prettier/recommended"
  ],
  "parserOptions": {
    "ecmaVersion": 2018,
    "sourceType": "module",
    "ecmaFeatures": {
      "impliedStrict": true
    }
  },
  "plugins": [
    "html"
  ]
}
```

Adjust the configuration according to your needs. For example code for Polymer 2, your `sourceType` is not `module`, and you don't need to lint `import/export` syntax, but you need to specify `Polymer` as a `global` and disallow overwriting.

Here is an example:

```json
{
  "env": {
    "browser": true,
    "es6": true
  },
  "parser": "babel-eslint",
  "extends": [
    "eslint:recommended",
    "plugin:prettier/recommended"
  ],
  "parserOptions": {
    "ecmaVersion": 2018,
    "ecmaFeatures": {
      "impliedStrict": true
    }
  },
  "plugins": [
    "html"
  ],
  "globals": {
    "Polymer": false
  }
}
```

For stylelint add this `.stylelintrc` file to your root:

```json
{
  "extends": [
    "stylelint-config-recommended",
    "stylelint-config-prettier"
  ],
  "plugins": [
    "stylelint-order"
  ],
  "rules": {
    "no-descending-specificity": null,
    "order/order": [
      "custom-properties",
      "declarations"
    ],
    "order/properties-alphabetical-order": true,
    "selector-type-no-unknown": [ true, {
      "ignoreTypes": [
        "/^[a-zA-Z]([a-zA-Z0-9]*-[a-zA-Z0-9]+)+/"
      ]
    } ]
  }
}
```

For Prettier add this `.prettierrc` file to your root:

```json
{
  "trailingComma": "all",
  "singleQuote": true
}
```

## package.json

Update the `package.json` file with the following settings to add the supported browsers and activate the pre-commit linting:

```
"browserslist": [
  "last 2 versions",
  "not ie <= 10"
],
"lint-staged": {
  "*.css": [
    "stylelint --fix",
    "git add"
  ],
  "*.html": [
    "stylelint --fix",
    "eslint --fix",
    "git add"
  ],
  "*.js": [
    "eslint --fix",
    "git add"
  ]
},
"scripts": {
  "precommit": "lint-staged"
}
```

Following is a `package.json` file with the minimum required settings:

```json
{
  "name": "<proper name>",
  "description": "<proper description>",
  "version": "<semantic versioning>",
  "engines": {
    "node": "<node SemVer version the code works on>"
  },
  "main": "<entry point>",
  "repository": {
    "type": "git",
    "url": "<repository url>"
  },
  "keywords": [
    "Arigo",
    "<proper keywords>"
  ],
  "contributors": [
    "<Full Name <email>>"
  ],
  "license": "UNLICENSED",
  "private": true,
  "bugs": {
    "url": "<repository url>/issues"
  },
  "homepage": "<repository url>#readme",
  "browserslist": [
    "last 2 versions",
    "not ie <= 10"
  ],
  "lint-staged": {
    "*.css": [
      "stylelint --fix",
      "git add"
    ],
    "*.html": [
      "stylelint --fix",
      "eslint --fix",
      "git add"
    ],
    "*.js": [
      "eslint --fix",
      "git add"
    ]
  },
  "scripts": {
    "precommit": "lint-staged"
  },
  "devDependencies": {
    "babel-eslint": "^8.2.3",
    "eslint": "^4.19.1",
    "eslint-config-prettier": "^2.9.0",
    "eslint-plugin-html": "^4.0.3",
    "eslint-plugin-import": "^2.12.0",
    "eslint-plugin-prettier": "^2.6.0",
    "husky": "^0.14.3",
    "lint-staged": "^7.1.0",
    "prettier": "^1.12.1",
    "stylelint": "^9.2.0",
    "stylelint-config-prettier": "^3.2.0",
    "stylelint-config-recommended": "^2.1.0",
    "stylelint-order": "^0.8.1"
  }
}
```

Adjust the `package.json` file to your needs, but if you remove something make sure you know what you are doing.

## Versioning

As a versioning convention use [Semantic Versioning](https://semver.org).
New to semantic versioning? [Learn the basics](https://docs.npmjs.com/getting-started/semantic-versioning).
For a great tool you can use to learn about how semver works with your favorite packages, see the [npm semver calculator](https://semver.npmjs.com).

### Increasing a Version

To increase the version of your packages, you need to invoke one of these commands:

```sh
# Let npm bump the version number
npm version <major|minor|patch>

# Same as previous except this time it generates a -<prerelease number> suffix. Example: v2.1.2-2.
npm version <premajor|preminor|prepatch|prerelease>
```

Invoking any of these updates `package.json` and creates a version commit to git automatically.
