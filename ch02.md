## Compilation

Ruby 1.9+ compiles your code. You don't use Ruby's compiler directly, it runs automatically.

**Note:** *No compiler for Ruby 1.8. It immediately executes your code after the tokenizing and parsing processes are finished.*

With Ruby 1.9, Koichi Sasada and the Ruby core team introduced Yet Another Ruby Virtual Machine ([YARV](https://en.wikipedia.org/wiki/YARV)), which actually executes your Ruby code.

- [Innovating the Ruby Interpreter](http://www.atdot.net/yarv/oopsla2005eabstract-rc1.pdf)

When using YARV you first compile your code into bytecode, a series of low-level instructions that the virtual machine understands.

Differences between YARV and the JVM:

- Ruby doesn't expose the compiler to you as a separate tool.
- Ruby never compiles your Ruby code all the way to machine language.

YARV is a stack-oriented virtual machine.

> It is not just a stack machine; it's a double-stack machine!

**Note:** *In Ruby all functions are actually methods. That is, functions are always associated with a Ruby class; there is always a receiver. Inside of Ruby, however, Ruby's parser and compiler distinguish between functions and methods: Method calls have an explicit receiver, while function calls assume the receiver is the current value of self.*

- [Ruby compiler](https://github.com/ruby/ruby/blob/trunk/compile.c)