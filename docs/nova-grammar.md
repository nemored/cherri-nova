# Nova Grammar

This document describes the Nova syntax definition at `Cherri.novaextension/Syntaxes/Cherri.xml`. Use it alongside Nova's Syntax Inspector to compare the grammar that is defined with the scopes Nova displays in the editor.

## Syntax Metadata

- Syntax identifier: `cherri`
- Display name: `Cherri`
- Language type: `script`
- Preferred file extension: `cherri`
- File detector: `.cherri` with priority `1.0`

## Editor Rules

Comments:

- Line comment: `//`
- Block comment: `/* ... */`

Bracket matching:

- `{ ... }`
- `[ ... ]`
- `( ... )`

Surrounding pairs:

- `{ ... }`
- `[ ... ]`
- `( ... )`
- `" ... "`
- `' ... '`

Indentation:

- Increase indentation after an unmatched opening `{`, `[`, or `(` before the end of the line.
- Decrease indentation when a line starts with optional whitespace followed by `}`, `]`, or `)`.

## Scope Evaluation Order

Nova includes these collections in this order:

1. `comments`
2. `double-quoted-strings`
3. `single-quoted-strings`
4. `keywords`
5. `pragmas`
6. `constants`
7. `types`
8. `declarations`
9. `functions`
10. `identifiers`

This order matters when more than one rule could match the same text. For example, `color` is currently both a keyword and a type; because `keywords` is included before `types`, Nova should scope `color` as `cherri.keyword`.

## Scope Reference

| Construct | Nova scope | Definition |
| --- | --- | --- |
| Block comment | `cherri.comment.block` | Starts with `/\*`, ends with `\*/` |
| Line comment | `cherri.comment.line` | `//.*$` |
| Double-quoted string | `cherri.string` | Starts with `"`, ends with `"` |
| Single-quoted string | `cherri.string` | Starts with `'`, ends with `'` |
| Escape sequence inside string | `cherri.character.escape` | `\\.` |
| Interpolation inside string | `cherri.string.interpolated` | `\{.*?\}` |
| Keyword | `cherri.keyword` | Keyword string list |
| Pragma | `cherri.processing` | `(#include\|#define\|#import\|#question)\b` |
| Language constant | `cherri.identifier.constant` | Constant string list |
| Numeric constant | `cherri.value.number` | `\b[0-9]+\b` |
| Type | `cherri.identifier.type` | Type string list |
| Declaration | `cherri.identifier.variable` | `@[^ ]+\b` |
| Function call | `cherri.identifier.function` | `\b[a-zA-Z_][a-zA-Z0-9_]*\b\(` |
| Identifier | `cherri.identifier.variable` | `\b[a-zA-Z_][a-zA-Z0-9_]*\b` |

## Keyword Strings

These are scoped as `cherri.keyword`:

```text
if
else
repeat
for
color
name
glyph
from
mac
inputs
noinput
askfor
getclipboard
menu
item
list
nil
const
action
in
stop
makeVCard
rawAction
embedFile
```

## Pragma Strings

These are scoped as `cherri.processing`:

```text
#include
#define
#import
#question
```

## Constant Strings

These are scoped as `cherri.identifier.constant`:

```text
CurrentDate
Device
RepeatIndex
RepeatItem
ShortcutInput
Ask
```

Numeric literals matching `\b[0-9]+\b` are scoped as `cherri.value.number`.

## Type Strings

These are scoped as `cherri.identifier.type`, unless an earlier collection matches first:

```text
text
number
bool
dictionary
array
variable
color
float
```

## Identifier-Like Matches

Declaration:

```text
@[^ ]+\b
```

Function call:

```text
\b[a-zA-Z_][a-zA-Z0-9_]*\b\(
```

Identifier:

```text
\b[a-zA-Z_][a-zA-Z0-9_]*\b
```

The function-call rule includes the opening parenthesis in the scoped range because the original TextMate grammar does the same.

## Example Probe

Use this sample in Nova with Syntax Inspector enabled:

```cherri
#include 'actions/scripting'
#include "shared.cherri"

// line comment
/*
block comment
*/

const count: number = 42
@MainAction
action greet(text name) {
    if Device {
        showAlert("Hello {name}\n")
    }
}
```

Expected notable scopes:

- `#include`: `cherri.processing`
- `"shared.cherri"` and `'shared.cherri'`: `cherri.string`
- `// line comment`: `cherri.comment.line`
- `/* ... */`: `cherri.comment.block`
- `const`, `action`, `if`: `cherri.keyword`
- `number`, `text`: `cherri.identifier.type`
- `42`: `cherri.value.number`
- `@MainAction`: `cherri.identifier.variable`
- `greet(` and `showAlert(`: `cherri.identifier.function`
- `Device`: `cherri.identifier.constant`
- `{name}` inside the string: `cherri.string.interpolated`
- `\n` inside the string: `cherri.character.escape`
