# eslint-plugin-brace-rules

Enhancements to eslint's default `brace-style` rules

## Installation

You'll first need to install [ESLint](http://eslint.org):

```
$ npm i eslint --save-dev
```

Next, install `eslint-plugin-brace-rules`:

```
$ npm install eslint-plugin-brace-rules --save-dev
```

**Note:** If you installed ESLint globally (using the `-g` flag) then you must also install `eslint-plugin-brace-rules` globally.

## Usage

Add `brace-rules` to the plugins section of your `.eslintrc` configuration file. You can omit the `eslint-plugin-` prefix:

```json
{
    "plugins": [
        "brace-rules"
    ]
}
```

Then, modify your `.eslintrc` config with the following updates:
- Disable the internal `brace-style` setting
- Set the default base rule-set that is close to your desired brace-style settings
- Add only the overrides you need from the list of [braceRuleProps](#braceRuleProps)

In the example below:
- All internal `brace-style` checks are disabled
- All `brace-rules/brace-on-same-line` violations are flagged as errors
- The baseline brace styling is `stroustrup`
- `brace on next-line` overrides are added for occurances of:
  - `class { ... }`
  - `export class MyClass { ... }`
  - `export default class MyClass { ... }`
  - `export default class { ... }`
  - `export default function { ... }`
```json
{
    "rules": {
        ...,
        "brace-style": 0,
        "brace-rules/brace-on-same-line": [
            "error",
            "stroustrup",
            {
                "ClassDeclaration": "never",
                "ExportClass": "never",
                "ExportClassAnon": "never",
                "ExportFunctionAnon": "never"
            }
        ],
        ...
    }
}
```

## Supported Rules

The basic schema for the `brance-on-same-line` is as follows:
```json
"brace-rules/brace-on-same-line": [
    ${standard eslint error levels, int/string-based},
(( schema ))
    {
        oneOf: [
            { enum: ["1tbs", "stroustrup", "allman"] },
            {
                type: "object",
                properties: { ...braceRuleProps }
            }
        ]
    },
    {
        type: "object",
        properties:
            ...braceRuleProps,
            allowSingleLine: { type: "boolean" },
            noCuddledElse: { type: "boolean" },
            noCuddledCatchFinally: { type: "boolean" }
        }),
        additionalProperties: false
    }
(( end schema ))
]
```
The anticipated flow here is to start with a base brace-style ruleset (i.e. `stroustrup`), and override it with specific rules (as shown in the example from the [Usage](#usage) section, above).

You can also completely define your brace-style rules on a per-keyword basis by specifying them all in the section following the error level. No additional braceStyle properties would need to be added in the third section, which overrides anything specified in the second section.

Properties that can be defined in the third section:
- **`...braceRuleProps`**: See below in [braceRuleProps](#braceRuleProps) for full details
- **`allowSingleLine`**: Allow braces on single-line declarations
    ```js
    if (foo) { return true; }
    ```
- **`noCuddledElse`**: Don't allow the `else` keyword to be on the same line as the closing brace of the preceeding `if` clause.
- **`noCuddledCatchFinally`**: Don't allow `catch` or `finally` keywords to be on the same line as the closing brace of the preceeding `try` and `catch` clauses (respectively).

### braceRuleProps
Below is the list of all brace rules that can be used. They all accept one of three values:
- **`always`**: Always require an opening brace on the same line as the keyword
- **`never`**: Never allow an opening brace on the same line as the keyword
- **`ignore`**: Ignore the bracing style for this keyword

Most of the brace rules are self-explanatary:
> "The `WithStatement` rule is for formatting braces around _**with**_ statements"

... but a few related to `export` need additional detail:

- **`ArrowFunctionExpression`**
- **`ClassDeclaration`**
- **`DoWhileStatement`**
- **`ExportClass`**: Adjusts brace formatting for `export class MyClass` or `export default class MyClass` declarations (overriding any _`ClassDeclaration`_ setting)
- **`ExportClassAnon`**: Adjusts brace formatting for `export default class` declarations (overriding any _`ClassDeclaration`_ and _`ExportClass`_ settings)
- **`ExportFunction`**: Adjusts brace formatting for `export function MyFunction` or `export default function MyFunction` declarations (overriding any _`FunctionDeclaration`_ setting)
- **`ExportFunctionAnon`**: Adjusts brace formatting for `export default function` declarations (overriding any _`FunctionDeclaration`_ and _`ExportFunction`_ settings)
- **`ForStatement`**
- **`ForInStatement`**
- **`ForOfStatement`**
- **`FunctionDeclaration`**
- **`FunctionExpression`**
- **`IfStatement`**
- **`SwitchStatement`**
- **`TryStatement`**
- **`WhileStatement`**
- **`WithStatement`**





