---
 title: Operators and Symbols
 category: Appendix
---

This appendix provides a glossary of Glu's syntax, encompassing operators
and symbols used independently or within paths, generics, macros, attributes, comments, and brackets.

## Operators
This table lists operators in Glu, along with examples of their contextual
usage, brief explanations, and indicates whether each operator is
overloadable. Operators are listed alongside their precedences.

| Operator | Example  | Explanation | Overloadable |
|----------|----------|-------------|--------------|
| `.` | `expr . ident` | Access to a member of a struct | false |
| `!` | `!expr` | Bitwise or logical complement | true |
| `? :` | `pat ? pat : pat` | Ternary pattern | false |
| `%` | `expr % expr` | Arithmetic remainder | true |
| `%` | `var %= expr` | Arithmetic remainder and assignment | true |
| `!=` | `expr != expr` | Nonequality comparison | true |
| `.*` | `expr.*`  | Dereference | true |
| `:` | `ident : type` | Specify type | false |
| `&` | `&expr` | Take the address of a value | false |
| `&` | `expr & expr` | Bitwise And | true |
| `&=` | `var &= expr` | Bitwise And and assignment | true |
| `&&` | `expr && expr` | Short-circuiting logical AND | true |
| `*` | `*type` | Raw pointer type | false |
| `*` | `*(...) -> type` | Function pointer type | false |
| `*` | `expr * expr` | Arithmetic multiplication | true |
| `*=` | `var *= expr` | Arithmetic multiplication and assignment | true |
| `+` | `expr + expr` | Arithmetic addition | true |
| `+=` | `var += expr` | Arithmetic addition and assignment | true |
| `/` | `expr / expr` | Arithmetic division | true |
| `/=` | `var /= expr` | Arithmetic division and assignment | true |
| `-` | `- expr` | Arithmetic negation | true |
| `-` | `expr - expr` | Arithmetic substraction | true |
| `-=` | `var -= expr` | Arithmetic substraction and assignment | true |
| `==` | `expr == expr` | Equality comparison | true |
| `||` | `expr || expr` | Short-circuiting logical OR | true |
| `>` | `expr > expr` | Greater then comparison | true |
| `>=` | `expr >= expr` | Greater then or equal comparison | true |
| `<` | `expr < expr` | Lesser then comparison | true |
| `<=` | `expr <= expr` | Lesser then or equal comparison | true |
| `->` | `func ident(...) -> type` | Function return type | false |
| `...` | `expr...` | Infinite Left-inclusive range literal | false |
| `...` | `expr...expr` | From expr to expr inclusive range literal | false |
| `..<` | `expr..<expr` | From expr to expr Right-exclusive range literal | false |
| `<<` | `expr << expr` | Left-shift | true |
| `<<=` | `var <<= expr` | Left-shift and assignment | true |
| `>>` | `expr >> expr` | Right-shift | true |
| `>>=` | `var >>= expr` | Right-shift and assignment | true |
| `^` | `expr ^ expr` | Bitwise exclusive OR | true |
| `^=` | `var ^= expr` | Bitwise exclusive OR and assignment | true |
| `|` | `expr | expr` | Bitwise OR | true |
| `|=` | `var |= expr` | Bitwise OR and assignment | true |
| `;` | `expr;` | statement terminator | false |
| `,` | `expr, expr` | Argument and elements separator | false |
| `[]` | `expr[expr]` | Increment and dereference pointer | true |

## Symbols
The following list includes all symbols that do not act as operators,
meaning they do not function like a call to a function.

| Symbol | Explanation |
|----------|----------|
| `"..."` | String literal |
| `'...'` | ASCII byte literal |
| `ident::ident` | Namespace path |
| `ident::ident` | Lib path |
| `func ident<ident>()` | Specify a generic type for a function |
| `path::*` | Wild card to import every modules from a path |
| `{...}` | Bloc expression |
| `Type {...}` | Struct literal |
| `Type[] = {...}` | Array literal |
