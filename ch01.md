## Tokenization and Parsing

Ruby reads and transforms your code 3 times (`code ---- tokenize -- (tokens) -- parse -- (AST nodes) -- compile ---- YARV instructions`) before running it.

Ruby's virtual machine is called "Yet Another Ruby Virtual Machine" (YARV).

### Tokens

The tokenization process transforms the source code (a stream of characters) into the words that make up the language.

- [parse.y](https://github.com/ruby/ruby/blob/trunk/parse.y) - Look at the `parser_yylex` function.
- [defs/keywords](https://github.com/ruby/ruby/blob/trunk/defs/keywords)

[Ripper](http://ruby-doc.org/stdlib-2.1.2/libdoc/ripper/rdoc/Ripper.html) has no idea whether the code you give it is valid Ruby or not. If you pass in code that contains a syntax error, Ripper will just tokenize it as usual and not complain. It's the parser's job to check syntax.

### Parsing

Words/tokens are grouped into sentences or phrases that make sense to Ruby.

Ruby uses a parser generator. Parser generators take a series of grammar rules as input that describe the expected order and patterns in which the tokens will appear. Ruby uses a newer version of Yacc (Yet Another Compiler Compiler) called [Bison](http://www.gnu.org/software/bison/) (a [LALR parser](http://en.wikipedia.org/wiki/LALR_parser) generator).

> LALR = Look-Ahead left-to-right, [rightmost derivation](http://en.wikipedia.org/wiki/Rightmost_derivation)

The grammar rule file is [parse.y](https://github.com/ruby/ruby/blob/trunk/parse.y). It contains the language definition.

Ruby runs Bison at build time to create the actual parser code (`Grammar rules, parse.y ---- Generate parser, Bison ---- Parser code, parse.c`).

**Note:** *The tokenization and parsing processes occur simultaneously.*

Ruby's `-y` options displays internal debug information every time the parser jumps from one state to another. It's useful for getting a sense of the complexity of Ruby's state table.

Display debug information about your code's AST using the `parsetree` option.

```
$ ruby --dump parsetree your_script.rb
```

**Programming Language Tools for Ruby**

- [parslet](http://kschiess.github.io/parslet/)
- [treetop](http://treetop.rubyforge.org/)