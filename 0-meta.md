[Ruby Under a Microscope](http://patshaughnessy.net/ruby-under-a-microscope)

## Introduction

To understand how Ruby works, read its [C source code](https://github.com/ruby/ruby). After learning each part of Ruby's internal implementation we perform an experiment and use Ruby to test itself.

Most of the book discusses how MRI works.

MRI (Matz's Ruby Interpreter) was invented in 1993 by Yukihiro Matsumoto a.k.a Matz.

Alternative implementations:

- [RubyMotion](http://www.rubymotion.com/) - *Write cross-platform apps for iOS, Android and OS X in Ruby*
- [MacRuby](https://github.com/MacRuby/MacRuby) - *An implementation of Ruby 1.9 directly on top of Mac OS X core technologies such as the Objective-C runtime and garbage collector, the LLVM compiler infrastructure and the Foundation and ICU frameworks*
- [IronRuby](http://ironruby.net/) - *An open-source implementation of the Ruby programming language which is tightly integrated with the .NET Framework*
- [Topaz](http://topaz.readthedocs.org/en/latest/) - *A high performance implementation of the Ruby programming language, written in Python on top of RPython*
- [JRuby](http://jruby.org/) - *A high performance, stable, fully threaded Java implementation of the Ruby programming language*
- [Rubinius](http://rubini.us/) - *An implementation of Ruby designed for concurrency using native threads to run Ruby code on all the CPU cores*
- [mruby](http://www.mruby.org/) - *A lightweight implementation of the Ruby language complying with part of the ISO standard*

The [JRuby](http://jruby.org/) and [Rubinius](http://rubini.us/) implementations are explored in detail in Chapters 10, 11 and 12.