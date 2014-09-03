im_writing
==========

The purpose of this repository is to hold ideas or documentation (but no code)
for a stack of programming languages I'm trying to invent.

The letters "im" in the repo name stood for Imperatrix Mundi, which I considered
for the name of one of the languages.

My first motivation for this project, historically, was to find a programming
language suitable for orthogonal persistency.  GemStone DBMS amounted to
Smalltalk on a disk, but Smalltalk has no natural transaction; moreover, it
is imperative.

In addition, I would like to have the same languages for programming a browser
as for programming a web server.  Programming the server in Ruby for example
and the browser in Javascript requires too much mental effort to switch
gears; also it prevents moving or copying code between browsers and servers.
And again, of course, those languages are imperative.

Three Languages
---------------

I imagine three programming languages:

- a functional language

- an actor language

- a language to underlie the other two.

The actor language will be able to call on the functional language.

I will define the semantics of the functional language and the actor language
in terms of the underlying language.

The underlying language need not be safe, but the higher-level languages must
be safe from at least one kind of logical failure, namely any attempt to bind
a logical variable more than once.  However, the high-level functional language
need not be statically provable as safe from non-termination, nor from running
out of memory for storing the state of evaluation.

The underlying language will be able to tell a story about itself as a pure
functional language surrounded by a runtime support that provides
Burton/Lennart "oracles" or something of similar power.  It is through
leveraging the oracles at the right times, that the underlying langauge will
be able to simulate the actor language and in particular determine the outcomes
that are not inherently determined by an actor language.  I will implement
"don't care" indeterminism for the actor language.

All three languages will be dynamically typed, in regard to the types of values
represented, but not necessarily in regard to the modes of the arguments in the
sense of in which directions data can flow and whether splitting and merging
are allowed and what such topological occurrences might mean.

The Functional Language
-----------------------

Everything that can be passed as an argument or returned as a result will
represent a pure value.  It will not be possible in the high-level functional
language to pass or return a reference, teller, bag channel, logical variable,
etc. of any kind.

Functions need not be strict in their arguments.

It will be possible for the programmer to define a class-like thing; let us
call it a "functional class" for now.

A functional class can be defined statically, as an artifact of the programming
process.

A functional class can contain functional methods.

Methods can be instance methods or creation methods.

For the time being, let us say that a class can define "instance variables".
Every creation method must populate all instance variables.

A functional class can inherit from other functional classes.

At runtime, it is possible to extract a "functional instance" of a functional
class.

A functional instance will be subject to method calls.

Functional instances will exhibit "duck typing".

Values can be copied and/or destroyed without restraint in all contexts in the
high-level functional language.

There will be literals for numbers, strings, etc.

Parameters will be named rather than positional.

A method may also be thought of as a rule.

To field a method call, the system must find a method whose head matches to
the call, or take some default for the absense of any match.

The pattern language for the matching of calls to methods is
characterized by the following assertions.

A method head includes a "verb", which is a word.

The method head can include mandatory parameters and optional parameters.

A parameter is a word.

When a call has the same verb as a method head, and supplies arguments whose
keywords match (equal) the mandatory parameters, and supplies no arguments
having keywords that are not parameters in the method head, then the
method head matches the call.  Otherwise, not.

To include more than one method in a class, that can match the same call, is
an error.

The order in which the programmer writes parameters or arguments with their
keywords does not signify.

A method body must end with an expression for the value that method will return.

Ahead of that final expression, it is permissible to define intermediate results
and give them names.

The order of the definitions does not matter.  A defined name can be used in a
definition and/or in the final expression.  Recursion is allowed.

If an operation gives rise to an error or an exception, the langauge expresses
this as a result with a value that carries the fact of the error or exception
along with information about the context in which the error or exception
occurred, including the location in the source code if feasible.

Probably I should include a closure construct.

There should be constructs to build arrays or lists or both, and records.  A
record can be the same thing as a message, i. e. the content of a call leaving
out the question of whom it is calling.  So a record would include a verb
and some keywords with arguments.  Folling Van Roy and Oz/Mozart, we can say
that a record without any keyword/argument pairs is the same thing as a symbol,
which would be given as simply the verb.  If we extend the idea of concrete
record to include an idea of a pattern record, the essence of a method head,
we need only distinguish mandatory keywords from optional ones.  However, it
is not my intent to put deep pattern matching in any of these langauges unless
I find for some reason (yet unknown at the time of this writing) that they
really can't work without it.

When the actor language tries to call the functional language, arguments are
checked to assure that they are acceptable to the functional language.

The Underlying Language
-----------------------

The underlying language is an unsafe concurrent constraint logical language
having one-shot logical variables.  All of the constraints that can be
asserted are equations (for example, there will be no primitive to constrain bag
membership).

Programs in the underlying language can include class definitions.

It is possible at runtime to extract an instance of a class.

The body of a method is a procedure rather than a function.

Method heads and matching of calls to methods operates in the same way, except
perhaps in regard to handling non-matches, as in the functional language.
However, where in the functional language, method calls appear in an expression
context, in the underlying language, method calls appear in a statement
context.  Such calls are to be understood as being asserted to be true.

A method in the underlying language is to be read as a rule of inference.
When the head is true, so are the statements in the body.  The body consists
of statements.  The order in which the statements are written in the body does
not signify.

Procedures need not be strict in their arguments.

The runtime environment in which a program executes will include logical I/O
devices.  A logical I/O device can connect a physical I/O device to the
conceptual millieu of the execution of the software.  A logical I/O device is
either an input device or an output device.  When it can be proven that a given
intermediate result can never affect any future output that can pass through
any logical output device, we can conclude that that intermediate result is
garbage, and we can forget about it, and collect the memory that it is using.
So, the logical output devices are the roots for garbage collection.

The runtime environment will supply objects carrying the capabilities necessary
to determine which of two communications or calculations finishes first, or
to determine the time taken by a communication or a calculation, or some
similar capabilities, as may be determined by need.  These oracles constitute a
sort of "god of the gaps".  The gaps they fill in are those between the inherent
capabilities of the underlying languae, which at bottom can assert nothing but
equations, and the necessary capabilities for simulating the actor language,
which needs to be able to write constraints about bag membership, which, you
may note, is not an equation.

[Stuck here, because in fact a bag-membership constraint can be written as
an equation, B2 = B1 U {e}.  I think I need to say something different about
the underlying language, than that its constraints are all equations.  I want
to explain that the underlying language is essentially a functional language.]

[I have in mind how to resolve the above bracketed consideration, but I am
stopping writing for the moment.]
