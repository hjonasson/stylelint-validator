[![NPM version](https://img.shields.io/npm/v/stylelint-csstree-validator.svg)](https://www.npmjs.com/package/stylelint-csstree-validator)
[![Build Status](https://github.com/csstree/stylelint-validator/actions/workflows/build.yml/badge.svg)](https://github.com/csstree/stylelint-validator/actions/workflows/build.yml)
[![Coverage Status](https://coveralls.io/repos/github/csstree/stylelint-validator/badge.svg?branch=master)](https://coveralls.io/github/csstree/stylelint-validator?branch=master)

# stylelint-csstree-validator

CSS syntax validator based on [csstree](https://github.com/csstree/csstree) as plugin for [stylelint](http://stylelint.io/). Currently it's only checking declaration values to match W3C specs and browsers extensions. It would be extended in future to validate other parts of CSS.

> Validator is designed to check CSS syntax only. However PostCSS (that used by stylelint as backend) may parse other syntaxes like Less or Sass and can be used for these syntaxes too. In this case validator is limited to check declaration that doesn't contain any CSS extension (e.g. variables).

## Install

```
$ npm install --save-dev stylelint-csstree-validator
```

## Usage

Setup plugin in your [stylelint config](http://stylelint.io/user-guide/configuration/):

```json
{
  "plugins": [
    "stylelint-csstree-validator"
  ],
  "rules": {
    "csstree/validator": true
  }
}
```

### Options

#### properties

Type: `Object` or `null`  
Default: `null`

Overrides or/and extends property definition dictionary. CSS [Value Definition Syntax](https://github.com/csstree/csstree/blob/master/docs/definition-syntax.md) is used to define value's syntax. If definition starts with `|` it added to existing definition if any. See [CSS syntax reference](https://csstree.github.io/docs/syntax/) for default definitions.

The following example extends `width` property and defines `size`:

```json
{
  "plugins": [
    "stylelint-csstree-validator"
  ],
  "rules": {
    "csstree/validator": {
      "properties": {
        "width": "| new-keyword | custom-function(<length>, <percentage>)",
        "size": "<length-percentage>"
      }
    }
  }
}
```

Using property definitions with the syntax `<any-value>` is an alternative for `ignore` option.

```json
{
  "plugins": [
    "stylelint-csstree-validator"
  ],
  "rules": {
    "csstree/validator": {
      "properties": {
        "my-custom-property": "<any-value>"
      }
    }
  }
}
```

#### types

Type: `Object` or `null`  
Default: `null`

Overrides or/and extends type definition dictionary. CSS [Value Definition Syntax](https://github.com/csstree/csstree/blob/master/docs/definition-syntax.md) is used to define value's syntax. If definition starts with `|` it added to existing definition if any. See [CSS syntax reference](https://csstree.github.io/docs/syntax/) for default definitions.

The following example defines new functional type `my-fn()` and extends `color` type:

```json
{
  "plugins": [
    "stylelint-csstree-validator"
  ],
  "rules": {
    "csstree/validator": {
      "properties": {
        "some-property": "<my-fn()>"
      },
      "types": {
        "color": "| darken(<color>, [ <percentage> | <number [0, 1]> ])",
        "my-fn()": "my-fn( <length-percentage> )"
      }
    }
  }
}
```

#### atrules

Type: `Object` or `null`  
Default: `null`

Overrides or/and extends atrule definition dictionary. Atrule's definition contains of `prelude` and `descriptors`, both are optional.

The following example defines new atrule `@example` with a prelude and two descriptors (a descriptor is the same as a declaration but with no `!important` allowed):

```json
{
  "plugins": [
    "stylelint-csstree-validator"
  ],
  "rules": {
    "csstree/validator": {
      "atrules": {
        "example": {
          "prelude": "<custom-ident>",
          "descriptors": {
            "foo": "<number>",
            "bar": "<color>"
          }
      }
    }
  }
}
```

#### ignore

Type: `Array` or `false`  
Default: `false`

Defines a list of property names that should be ignored by the validator.

```json
{
  "plugins": [
    "stylelint-csstree-validator"
  ],
  "rules": {
    "csstree/validator": {
      "ignore": ["composes", "foo", "bar"]
    }
  }
}
```

In this example, plugin would not test declaration with property name `composes`, `foo` or `bar`. As a result, no warnings for these declarations.

#### ignoreValue

Type: `RegExp`  
Default: `false`

Defines a pattern for values that should be ignored by the validator.

```json
{
  "plugins": [
    "stylelint-csstree-validator"
  ],
  "rules": {
    "csstree/validator": {
      "ignoreValue": "^pattern$"
    }
  }
}
```

In this example, the plugin will not report warnings for values that match the given pattern. Warnings will still be reported for properties.

## License

MIT
