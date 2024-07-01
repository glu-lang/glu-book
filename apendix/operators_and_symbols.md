# Operators and Symbols
This appendix provides a glossary of Glu's syntax, encompassing operators
and symbols used independently or within paths, generics, trait bounds,
macros, attributes, comments, tuples, and brackets.

## Operators
This table lists operators in Glu, along with examples of their contextual
usage, brief explanations, and indicates whether each operator is
overloadable. Operators are listed alongside their precedences.

| Operator | Example  | Explanation | Overloadable |
|----------|----------|-------------|--------------|
| `.` | `expr . ident` | access to a member of a struct | false |
| `.*` | `expr .* ident`  | access to a member of a struct after dereferencing | false |
| `:` | `ident : type` | specify type | false |
| `&` | `&expr` | Borrow | false |
| `*` | `*expr` | Dereference | true |
| `*` | `let ident: *type` | Raw pointer | false |
| `*` | `expr * expr` | Arithmetic multiplication | true |
| `*=` | `var *= expr` | Arithmetic multiplication and assignement | true |
| `+` | `expr + expr` | Arithmetic addition | true |
| `+=` | `var + expr` | Arithmetic addition and assignement | true |
| `/` | `expr / expr` | Arithmetic division | true |
| `/=` | `var / expr` | Arithmetic division and assignement | true |
| `-` | `- expr` | Arithmetic negation | true |
| `-` | `expr - expr` | Arithmetic substraction | true |
| `-=` | `var - expr` | Arithmetic substraction and assignement | true |
| `==` | `expr == expr` | Equality comparison | true |
| `&&` | `expr && expr` | Short-circuiting logical AND | true |
| `\|\|` | `expr \|\| expr` | Short-circuiting logical OR | true |
| `>` | `expr > expr` | Greater then comparison | true |
| `>=` | `expr >= expr` | Greater then comparison or equal | true |
| `<` | `expr < expr` | Lesser then comparison | true |
| `<=` | `expr <= expr` | Lesser then or equal comparison | true |
| `->` | `fn(...) -> type` | Function type | false |
| `!` | `expr!` | Bitwise or logical complement | true |
