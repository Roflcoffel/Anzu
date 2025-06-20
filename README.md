# 🍊 Anzu Programming Language 
This language follows this tutorial: [https://www.craftinginterpreters.com/the-lox-language.html](Crafting Interpreters)
but instead of java, Anzu will be in C#.

## TODO (Tree-Walking Intepreter)
- [ ] Scanner                (Ch. 4 )
- [ ] Representing Code      (Ch. 5 )
- [ ] Parsing Expressions    (Ch. 6 )
- [ ] Evaluating Expressions (Ch. 7 )
- [ ] Statments and State    (Ch. 8 )
- [ ] Control Flow           (Ch. 9 )
- [ ] Functions              (Ch. 10)
- [ ] Resolving and Binding  (Ch. 11) 
- [ ] Classes                (Ch. 12)
- [ ] Inheritence            (Ch. 13)

## TODO II (Standard Library)
- [ ] Math Library
- [ ] String Manipulation
- [ ] IO Library

## Anzu Language Specification
Will follow Lox for now and divate where possible.

I want Anzu to be simple to type, meaning a for loop like this:
```C
for(int i=1;i<10;i++) {}
```
that is too complicated i think and could be just:
```Lua
loop 10 end
```
as the most basic version, with the possiblity of doing something more complicated like this:
```Lua
loop 1 10 2 end
```

the first param is:  min with a default of 0
the second param is: max
the third param is:  sep with a default of 1

and to access the current index we use i, in inner loop would then have j.
the names can be changed by changing loop.vars which contains list of loop variables.

---

I want Anzu to avoid using parentheses so a function declaration would go from:
```C
void test(int a, int b) {}
```
and in Anzu:
```Lua
fun test a,b end
```
with an return type:
```Lua
fun test a,b : int end
```
and calling a function is:
```Lua
test! 10 10
```
---

if, else are the similar to Lua
```Lua
    -- Lua
    if a < 10 then
        print("Less than 10")
    else
        print("More than 10")
    end
```
in Anzu this would be:
```Lua
if a < 10:
    print "Less than 10"
else
    print "More than 10"
end
```

## Dictionary Of Anzu
loop, fun, end, ret, if, else, elif, not, and, or

## Anzu Syntax Grammar
```EBNF
program        → declaration* EOF ;
```
### Declarations
```EBNF
declaration    → classDecl
               | funDecl
               | varDecl
               | statement ;

classDecl      → "class" IDENTIFIER ( "<" IDENTIFIER )?
                 "{" function* "}" ;
funDecl        → "fun" function ;
varDecl        → "var" IDENTIFIER ( "=" expression )? ";" ;
```

### Statments
```EBNF
statement      → exprStmt
               | forStmt
               | ifStmt
               | printStmt
               | returnStmt
               | whileStmt
               | block ;

exprStmt       → expression ";" ;
forStmt        → "for" "(" ( varDecl | exprStmt | ";" )
                           expression? ";"
                           expression? ")" statement ;
ifStmt         → "if" "(" expression ")" statement
                 ( "else" statement )? ;
printStmt      → "print" expression ";" ;
returnStmt     → "return" expression? ";" ;
whileStmt      → "while" "(" expression ")" statement ;
block          → "{" declaration* "}" ;
```

### Expressions
```EBNF
expression     → assignment ;

assignment     → ( call "." )? IDENTIFIER "=" assignment
               | logic_or ;

logic_or       → logic_and ( "or" logic_and )* ;
logic_and      → equality ( "and" equality )* ;
equality       → comparison ( ( "!=" | "==" ) comparison )* ;
comparison     → term ( ( ">" | ">=" | "<" | "<=" ) term )* ;
term           → factor ( ( "-" | "+" ) factor )* ;
factor         → unary ( ( "/" | "*" ) unary )* ;

unary          → ( "!" | "-" ) unary | call ;
call           → primary ( "(" arguments? ")" | "." IDENTIFIER )* ;
primary        → "true" | "false" | "nil" | "this"
               | NUMBER | STRING | IDENTIFIER | "(" expression ")"
               | "super" "." IDENTIFIER ;
```

### Utility Rules
```EBNF
function       → IDENTIFIER "(" parameters? ")" block ;
parameters     → IDENTIFIER ( "," IDENTIFIER )* ;
arguments      → expression ( "," expression )* ;
```

### Lexical Grammar
```EBNF
NUMBER         → DIGIT+ ( "." DIGIT+ )? ;
STRING         → "\"" <any char except "\"">* "\"" ;
IDENTIFIER     → ALPHA ( ALPHA | DIGIT )* ;
ALPHA          → "a" ... "z" | "A" ... "Z" | "_" ;
DIGIT          → "0" ... "9" ;
```