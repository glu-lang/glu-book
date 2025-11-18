---
 title: Operators and Literals
 category: Appendix
---

This appendix provides a glossary of Glu's syntax, encompassing operators and literals that can be used as an expression in Glu code.

## Operators
This table lists operators in Glu, along with examples of their contextual
usage, brief explanations, and indicates whether each operator is
overloadable. The operators are listed by precedence, from lowest to highest.

| Operator | Precedence Group | Example  | Explanation | Overloadable |
|----------|------------------|----------|-------------|--------------|
| `? :` | Ternary | `bool ? true : false` | Ternary pattern | No |
| `||` | Logical OR | `a || b` | Short-circuiting logical OR | No |
| `&&` | Logical AND | `a && b` | Short-circuiting logical AND | No |
| `==` | Equality | `a == b` | Equality comparison | `func ==(a: T1, b: T2) -> T3` |
| `!=` | Equality | `a != b` | Nonequality comparison | `func !=(a: T1, b: T2) -> T3` |
| `>` | Comparison | `a > b` | Greater then comparison | `func >(a: T1, b: T2) -> T3` |
| `>=` | Comparison | `a >= b` | Greater then or equal comparison | `func >=(a: T1, b: T2) -> T3` |
| `<` | Comparison | `a < b` | Lesser then comparison | `func <(a: T1, b: T2) -> T3` |
| `<=` | Comparison | `a <= b` | Lesser then or equal comparison | `func <=(a: T1, b: T2) -> T3` |
| `...` | Range | `a...b` | From a to b inclusive range literal | `func ...(a: T1, b: T2) -> T3` |
| `..<` | Range | `a..<b` | From a to b right-exclusive range literal | `func ..<(a: T1, b: T2) -> T3` |
| `+` | Additive | `a + b` | Arithmetic addition | `func +(a: T1, b: T2) -> T3` |
| `-` | Additive | `a - b` | Arithmetic substraction | `func -(a: T1, b: T2) -> T3` |
| `|` | Additive | `a | b` | Bitwise OR | `func |(a: T1, b: T2) -> T3` |
| `^` | Additive | `a ^ b` | Bitwise XOR | `func ^(a: T1, b: T2) -> T3` |
| `*` | Multiplicative | `a * b` | Arithmetic multiplication | `func *(a: T1, b: T2) -> T3` |
| `/` | Multiplicative | `a / b` | Arithmetic division | `func /(a: T1, b: T2) -> T3` |
| `%` | Multiplicative | `a % b` | Arithmetic remainder | `func %(a: T1, b: T2) -> T3` |
| `&` | Multiplicative | `a & b` | Bitwise AND | `func &(a: T1, b: T2) -> T3` |
| `<<` | Shift | `a << b` | Left-shift | `func <<(a: T1, b: T2) -> T3` |
| `>>` | Shift | `a >> b` | Right-shift | `func >>(a: T1, b: T2) -> T3` |
| `as` | Cast | `a as T1` | Type cast | `func as(a: T1) -> T3` (Planned) |
| `+` | Unary | `+ a` | Arithmetic no-op | `func +(a: T1) -> T2` |
| `-` | Unary | `- a` | Arithmetic negation | `func -(a: T1) -> T2` |
| `!` | Unary | `!a` | Logical negation | `func !(a: T1) -> T2` |
| `~` | Unary | `~a` | Bitwise complement | `func ~(a: T1) -> T2` |
| `&` | Unary | `&a` | Take the address of a variable | No |
| `.` | Postfix | `a.member` | Access to a member of a struct | No |
| `.*` | Postfix | `a.*`  | Dereference | `func .*(a: T1) -> T2` |
| `[]` | Postfix | `a[b]` | Array element access | `func [](a: T1, b: T2) -> T3` |
| `()` | Postfix | `a(b, c, d, ...)` | Function call | No |

## Literals
The following list includes all literals and their explanations.

| Literal | Explanation |
|----------|----------|
| `"abc"` | String or Character literal |
| `{...}` | Aggregate Initializer (Struct, Arrays, ...) |
| `0b123` | Binary integer literal |
| `0o123` | Octal integer literal |
| `0x123` | Hexadecimal integer literal |
| `123` | Decimal integer literal |
| `123.45` | Floating-point literal |
| `true` | Boolean true literal |
| `false` | Boolean false literal |
| `null` | Null pointer literal |
