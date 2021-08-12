> what seems complex from a distance is often quite simply when you look closely enough

Ruby is dynamic; using metaprogramming, Beneath this thin veneer of simplicity, Ruby is a very complex tool.

> you’ll discover that a few important computer science concepts underpin Ruby’s many features, you’ll gain a deeper understanding of what is happening under the hood as you use the language.

> conceptual understanding of how Ruby works without your having to understand how to program in C.

> It doesn’t matter how beautiful your theory is, it doesn’t matter how smart you are. If it doesn’t agree with experiment, it’s wrong. —Richard Feynman.

we need to perform experiments to be sure our hypotheses are correct.

# In Chapter 1: Tokenization and Parsing, you’ll learn how Ruby parses your Ruby program. 

* Your code has a long road to take before Ruby ever runs it.

* Ruby rips your code apart into small pieces and then puts them back together in a different format three times! Between the time you type ruby and the time you start to see actual output on the console, your Ruby code has a long road to take.

* Ruby tokenizes your code, which means it reads the text characters in your code file and converts them into tokens, the words used in the Ruby4 Chapter 1 language. Next, Ruby parses these tokens; that is, it groups the tokens into meaningful Ruby statements just as one might group words into sentences. Finally, Ruby compiles these statements into low-level instructions that it can execute later using a virtual machine.

* first thing Ruby does is open simple.rb and read in all the text from the code file. Next, it needs to make sense of this text: your Ruby code.

* Ruby encounters the series of text characters.

* it converts them into a series of tokens or words that it understands by stepping through the characters one at a time

* The Ruby C source code contains a loop that reads in one character at a time and processes it based on what that character is. To keep things simple, I’m describing tokenization as an independent process. In fact, the parsing engine I describe next calls this C tokenize code whenever it needs a new token. Tokenization and parsing are separate processes that actually occur at the same time

* ruby realizes that the character 1 is the start of a number and continues to iterate over the characters that follow until it finds a nonnumeric character

* Ruby converts the numeric characters that it found into the first token from your program, called tINTEGER

* Ruby encounters the word times and creates an identifier token.

* Identifiers are words in your Ruby code that are not reserved words.

* Identifiers usually refer to variable, method, or class names.

* Next, Ruby sees do and creates a reserved word token, as indicated by keyword_do 

* Reserved words are keywords that carry significant meaning in Ruby because they provide the structure, or framework, of the language

* ruby has processed your code for the first time

* Your code began as a stream of text characters, and Ruby converted it to a stream of tokens words that it will later combine into sentences.


* Ripper makes it very easy to see what tokens Ruby creates for different code files. Shipped with Ruby 1.9 and Ruby 2.0, the Ripper class allows you to call the same tokenization and parsing code that Ruby uses to process text from code files.
```
Example:
$ ruby lex1.rb
10.times do |n|
puts n
end
1-> [[[1, 0], :on_int, "10"],10 Chapter 1
[[1, 2], :on_period, "."],
2-> [[1, 3], :on_ident, "times"],
[[1, 8], :on_sp, " "],
[[1, 9], :on_kw, "do"],
[[1, 11], :on_sp, " "],
[[1, 12], :on_op, "|"],
[[1, 13], :on_ident, "n"],
[[1, 14], :on_op, "|"],
[[1, 15], :on_ignored_nl, "\n"],
[[2, 0], :on_sp, " "],
[[2, 2], :on_ident, "puts"],
[[2, 6], :on_sp, " "],
[[2, 7], :on_ident, "n"],
[[2, 8], :on_nl, "\n"],
[[3, 0], :on_kw, "end"],
[[3, 3], :on_nl, "\n"]]
```
* Each line corresponds to a single token that Ruby found in your code string. On the left, we have the line number (1, 2, or 3 in this short example) and the text column number. Next, we see the token itself displayed as a symbol, such as :on_int 1 or :on_ident 2

* It’s the parser’s job to check syntax.

* Does Ruby simply step through the tokens and execute each one in order?

* parsing, where words or tokens are grouped into sentences or phrases that make sense to Ruby. When parsing, Ruby takes into account the order of operations, methods, blocks, and other larger code structures.

* Ruby uses a parser generator. Ruby uses a parser to process tokens, but the parser itself is generated with a parser generator. Parser generators take a series of grammar rules as input that describe the expected order and patterns in which the tokens will appear.

* The most widely used and well-known parser generator is Yacc (Yet Another Compiler Compiler), but Ruby uses a newer version of Yacc called Bison. The grammar rule file for Bison and Yacc has a .y extension

* The parse.y file defines the actual syntax and grammar that you have to use while writing your Ruby code; it’s really the heart and soul of Ruby and where the language itself is actually defined! Ruby uses an LALR parser

* The parse.y file defines the actual syntax and grammar that you have to use while writing your Ruby code; it’s really the heart and soul of Ruby and where the language itself is actually defined! Ruby uses an LALR parser

* the Ruby build process uses Bison to generate the parser code (parse.c) from the grammar rule file (parse.y). Later, at run time, this generated parser code parses the tokens returned by Ruby’s tokenizer code

* Because the parse.y file and the generated parse.c file also contain the tokenization code

* (In fact, the parse engine I’m about to describe calls the tokenization code whenever it needs a new token.) The tokenization and parsing processes actually occur simultaneously.

* How does the parser code analyze and process the incoming tokens? With an algorithm known as LALR, or Look-Ahead Left Reversed Rightmost Derivation. Using the LALR algorithm, the parser code processes the token stream from left to right, trying to match their order and the pattern in which they appear against one or more of the grammar rules from parse.y. The parser code also “looks ahead” when necessary to decide which grammar rule to match.

* you use Bison to generate a C language parser from a grammar file, you can write the simple grammar, with the rule name on the left and the matching tokens on the right.

* If the token stream is equal to me, gusta, el, and ruby—in that order—we have a match. If there’s a match, the Bison generated parser will run the given C code, and the printf statement (similar to puts in Ruby) 

* We have a match on the SpanishPhrase rule.

Example:
```
SpanishPhrase: VerbAndObject el ruby {
printf("%s Ruby\n", $1);
};
VerbAndObject: SheLikes | ILike {
$$ = $1;
};
SheLikes: le gusta {
$$ = "She likes";
}
ILike: me gusta {
$$ = "I like";
}
```
* Also, you’re using the Bison directive $$ to return a value from a child grammar rule to a parent and $1 to refer to a child’s value from a parent

* the acronym LALR stands for Look-Ahead LR parser, and it16 Chapter 1 describes the algorithm the parser uses to find matching grammar rules. We’ll get to the look ahead part in a minute. For now, let’s start with LR:
•	 L (left) means the parser moves from left to right while processing the token stream. In this example, that would be le, gusta, el, and ruby, in that order.
•	 R (reversed rightmost derivation) means the parser takes a bottom-up strategy, using a shift/reduce technique, to find matching grammar rules


* This operation is called reduce because the parser is replacing the pair of tokens with a single matching rule. The parser looks through the available rules and reduces, or applies the single matching rule. Now the parser can reduce again because there’s another matching rule: VerbAndObject! The VerbAndObject rule matches because its use of the OR (|) operator matches either the SheLikes or ILike child rules. You can see in Figure 1-20 that the parser replaces SheLikes with VerbAndObject.

