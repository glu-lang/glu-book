---
title: Glu Formal Grammar
category: The Architecture
---

The following is the formal grammar of the Glu programming language.

## Definitions

- Non-terminal symbols are enclosed in angle brackets `< >`.
- Terminal symbols are enclosed in single quotes `' '`.
- A question mark `?` denotes an optional element.
- An asterisk `*` denotes zero or more repetitions.
- A plus sign `+` denotes one or more repetitions.
- The vertical bar `|` denotes a choice between alternatives.
- Parentheses `()` are used for grouping.
- Two dots `..` are used to denote a range of ASCII characters.
- Whitespace is not significant except to separate tokens.
- An equal sign `=` is used to define a rule.

## Whitespace

Whitespace is used to separate tokens and is not significant in the grammar.

```
whitespace = whitespace_item*
whitespace_item = line_comment | block_comment | newline | space
line_comment = '//' line_comment_text* newline
block_comment = '/*' block_comment_text* '*/'
line_comment_text = (Any character except newline)
block_comment_text = (block_comment | Any character except '*/')*
newline = '\n' | '\r'
space = (Any character in the Unicode Space general category)
```

## Identifiers

Identifiers are used to name variables, functions, and other entities in the program.

```
identifier = ticked_identifier | simple_identifier
ticked_identifier = '`' ticked_identifier_text* '`'
ticked_identifier_text = ('``' | Any character except '`')
simple_identifier = letter (letter | digit | '_')*
letter = (Any character in the Unicode Letter general category)
digit = (Any character in the Unicode Decimal Number general category)

namespaced_identifier = identifier ('::' identifier)*
```

## Literals

Literals are used to represent constant values in the program.

```
literal = boolean_literal | integer_literal | float_literal | string_literal

boolean_literal = 'true' | 'false'

integer_literal = decimal_literal | hexadecimal_literal | binary_literal | octal_literal

decimal_literal = decimal_digit+
decimal_digit = ('0' .. '9')

hexadecimal_literal = '0x' hexadecimal_digit+
hexadecimal_digit = ('0' .. '9' | 'a' .. 'f' | 'A' .. 'F')

binary_literal = '0b' binary_digit+
binary_digit = '0' | '1'

octal_literal = '0o' octal_digit+
octal_digit = ('0' .. '7')

float_literal = decimal_literal '.' decimal_literal?

string_literal = '"' string_literal_text* '"'
string_literal_text = (string_escape_sequence | Any character except '"')
string_escape_sequence = '\' ( '"' | '\'' | '\\' | 'n' | 'r' | 't' | 'u' hex_digit+ )
```

## Top-Level Definitions

Top-level definitions are used to define the structure of the program.

```
document = top_level*
top_level = import_declaration | type_declaration | function_declaration

attributes = attribute*
attribute = '@' identifier

import_declaration = 'import' import_path ';'
import_path = (identifier '::')* import_item
import_item = identifier | '*' | '{' import_item_list? '}'
import_item_list = import_path (',' import_path)* ','?

type_declaration = struct_declaration | enum_declaration | typealias_declaration

struct_declaration = attributes 'struct' identifier template_definition? struct_body
struct_body = '{' struct_field (',' struct_field)* ','? '}'
struct_field = identifier ':' type

enum_declaration = attributes 'enum' identifier ':' type enum_body
enum_body = '{' enum_variant (',' enum_variant)* ','? '}'
enum_variant = identifier ('=' expression)?

typealias_declaration = attributes 'typealias' identifier template_definition? '=' type ';'

template_definition = '<' template_parameter (',' template_parameter)* ','? '>'
template_parameter = identifier

function_declaration = attributes 'func' identifier template_definition? function_signature function_body
function_body = block | ';'
function_signature = '(' parameter_list? ')' ('->' type)?
parameter_list = parameter (',' parameter)* ','?
parameter = identifier ':' type ('=' expression)?
```

## Statements

Statements are used to perform actions in the program.

```
statement = block | expression_stmt | var_stmt | let_stmt | return_stmt | if_stmt | while_stmt | for_stmt | break_stmt | continue_stmt | empty_stmt | assignment_stmt

block = '{' statement* '}'
empty_stmt = ';'
expression_stmt = expression ';'

var_stmt = 'var' identifier (':' type)? ('=' expression)? ';'
let_stmt = 'let' identifier (':' type)? '=' expression ';'

return_stmt = 'return' expression? ';'

if_stmt = 'if' expression block ('else' statement)?
while_stmt = 'while' expression block
for_stmt = 'for' identifier 'in' expression block
break_stmt = 'break' ';'
continue_stmt = 'continue' ';'

assignment_stmt = expression assignment_operator expression ';'
```

## Expressions

Expressions are used to compute values in the program.

```
expression = literal | identifier | function_call | binary_expression | unary_expression | paren_expression | initializer_list | ternary_expression | field_access | subscript | cast_expression

function_call = namespaced_identifier template_arguments? '(' argument_list? ')'
argument_list = expression (',' expression)* ','?
template_arguments = '<' type (',' type)* ','? '>'

binary_expression = expression binary_operator expression
binary_operator = '+' | '-' | '*' | '/' | '%' | '==' | '!=' | '<' | '<=' | '>' | '>=' | '&&' | '||' | '&' | '|' | '^' | '<<' | '>>' | '...' | '..<'

unary_expression = unary_operator expression
unary_operator = '+' | '-' | '!' | '~' | '&'

paren_expression = '(' expression ')'

initializer_list = '{' expression (',' expression)* ','? '}'

ternary_expression = expression '?' expression ':' expression

field_access = expression '.' (identifier | '*')
subscript = expression '[' expression ']'

cast_expression = expression 'as' type
```

## Types

Types are used to specify the kind of data that can be stored in variables.

```
type = simple_type | function_type | array_type | pointer_type

simple_type = namespaced_identifier template_arguments?
function_type = '(' (type (',' type)* ','?)? ')' '->' type
array_type = type '[' integer_literal ']'
pointer_type = '*' ('unique' | 'shared')? type
```
