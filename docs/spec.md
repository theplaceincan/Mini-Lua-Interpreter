## Defining "Mini-Lua"
This language will, for now, only perform print() and arithmetic Here's informtion about the language and how it is defined in this light interpreter.

#

### 1. Lexical Definition
* **White Space**: spaces, tabs, newlines, - <ins>ignored except inside strings.
* **Comments**: `--` to end of line - <ins>ignored
* **Tokens**:
  * **INTEGER**: `0 | [1-9][0-9]` (no sign) 
  * **STRING**: `"..."` or `'...'` with escapes `\"` `\'` `\\` and optionally `\n`
  * **IDENTIFIER**: `[A-Za-z_][A-Za-z0-9_]*`
  * **KEYWORDS**: `print` (CASE SENSITIVE)
  * **OPERATORS**: `+` `-` `/` `*` (LEFT-ASSOCIATIVE)
  * **PUNCTUATION**: `(` `)` `,`
* **Unknown character**: good ol' lex error

### 2 Error types:
1. Runtime: type mismatch, divide-by-zero
2. Lex: unknown char, unterminated string
3. Parse: missing or unexpected token

### 3. Recognized Symbols
* ` " `
* ` ' `
* ` `
* ` * `
* ` / `
* ` + `
* ` - `
* ` ( `
* ` ) `
* ` , `

### 4. BNF-defined Grammar
*Note: print_statement will allow only 1 expression* <br>
*Note: statements are separated by a ; (the ONLY deviation from Lua)* <br>

> program := {statement} EOF <br>
statement := print_statement <br>
print_statement := `"print"` `(` expression `)`<br>
expression := <br>
   -unary := `"-"` unary | primary<br>
   -multiplicative := unary { (`"*"` | `"/"`) unary } <br>
   -additive := multiplicative { (`"+"` | `"-"`) multiplicative } <br>
   -primary := INTEGER | STRING | `"("` expression `")"` <br>


### 5. Semantics
1. Expressions are evaluated left->right
2. print(e) evaluates e, converts to text, writes the text to the console, then sends a new line. Anything not an integer requires `"..."` to be a string inside print().
3. Airhtmetic operators apply to integers only in v0 (`+` is not string concatenation)
4. Integer division is truncated toward 0. x/0 is a runtime error.
4. Unary `-` negates an integer

Example: <br>

_Program.mlu_:
```
print("Hello world");
print(69420);
print(3+2);
```
_Output_:
```
Hello world
69420
5
```
