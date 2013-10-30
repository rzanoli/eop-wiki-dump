This page describes the general coding standard for EXCITEMENT open platform codes. 

## Documentation
* Every class / interface / enum should include a main comment in its beginning, describing:
   1. the purpose of the class
   2. its contents
   3. its usage
   4. and how it is related to other classes.

* The main comment should include also the author name (first name and surname, with no abbreviations) and the date when the class was created first.

* Each method must be paired with a comment describing its purpose, usage, parameters, return value. In particular, the comment must make explicit as any policies on its use beyond its type signature.

* The class and functions (methods) comments specified above should be in Java-Doc format. (Tip: in Eclipse use Alt+Shift+J to generate a javadoc skeleton.) 

* Comments should be written in the code itself, describing the flow, i.e. describing what the code does. It is required when the code is not clear (i.e. self explaining), and recommended for any long code (long code = code with more that 5 lines).

* The best practice is writing in such a way that a programmer that will read your code will be able to understand it, without additional explanations.


## Error Handling

* Make sure you're familiar with the concept of Checked Exceptions in Java [[http://en.wikibooks.org/wiki/Java_Programming/Checked_Exceptions|http://en.wikibooks.org/wiki/Java_Programming/Checked_Exceptions]]

* We will use a single way to handle errors: throwing exceptions.

* Code should not write to System.err.

* Code should not call System.exit().

* Writing to System.err is allowed only in the module that handles the very beginning and very end of the flow (i.e. the class that contains the main() method, and may be one or two other classes that are called by it).

* Calling System.exit() is allowed only for GUI applications, and only in the module that handles the GUI events.

* RuntimeException and its subclasses should never be thrown explicitly. It is also recommended to wrap implicit potential throws of subclasses of RuntimeException by try...catch that throws a subclass of Exception that is not a runtime exception.

* When throwing an exception, include a string in the exception that describes the problem, and how it can be fixed.

* "Stopping" an exception is usually a very bad idea. "Stopping" means is catching it somewhere and not throwing it (or another exception) again. The problem is that the user will not be aware of underlying the problem that caused the first exception to be thrown.

* Handling exceptions that are thrown from an inner components can be done in two ways:
   1. Catch them, and throw a new exception that wraps the original ones.
   2. Let them be thrown up in the call-stack.

## Naming Conventions

* Package names should be in lower case letters only. Even a multi-word name should not include any upper-case letter.
* Class names must start with an upper case letter.
* Function names and variable names must start with lower case letter.
* Constant names must include upper-case letters only, No lower case letters are allowed in constant names. The underscore (_) character can be used to separate words in constant names. 
* Use meaningful names for everything.

## Writing Good Code

* Local variables should be declared in the inner-most possible block. Don't declare local variables in advance, but ad-hoc. This convention helps eliminating some hard-to-observe bugs.
   1.Nevertheless, for the sake of saving many calls to a costly constructor, it may be wise to declared a local variable out of its minimal scope. 

* Write short code.
   1. Classes should be short. Try writing classes that are no longer than 300 lines. A long class is an evidence to poor design. A long class is usually a class that had to be created as several classes, in a hierarchical way.
   2. Functions should be short - no more than 25 lines. A long function is hard to understand, and is an evidence that one function does too many things, that had to be partitioned into several functions (most of them non-public).

* Do not use nested classes. Nested classes are required only in rare cases, where the nested class needs access to its parent's private members , should not be known outside, and is logically part of the parent, but needs also its own private context. Those cases are rare.

* Do not use nested static classes. You can use them only sometimes for declaring specific exceptions, or in some rare cases. In general, using nested classes, either static or non-static, makes a hard-to-understand and hard-to-change code.

* Make sure your code has no compilation warnings. Compilation warnings are a good tool for avoiding bugs. A code that contains warning makes them unusable.

* It is strongly recommended that every class will contain all of its "public" constructors / method and fields together. Putting all of the public stuff at the beginning of the class, with a clear comment separator between public and non-public part, makes the class easier to understand and use.
   1. It's convenient to change eclipse's settings to place new generated private methods at the bottom of the class.

* Use constants. Numbers and strings should not be hard coded in the code itself, but as constants.
Put all the constants together at the beginning of the class, and make them "final".

* Never use early-access code or any code that may become incompatible in future environment. Practically, do not depend on any beta/snapshot jars and artifacts. 

* For abstract data types, that is, classes that represent some non-atomic information as a single object, make sure that the following conditions are met:
   1. Abstract data types should typically implement their own "equals" and "hashCode" methods. Make sure that they are implemented if necessary.
   2. Make sure you do not implement those methods when they should not be implemented.
   3. Make sure you implement them correctly. Eclipse has a default way to implement those methods (source--> insert --> hashcode feature). Use it. Do not use another implementation unless you know what you are doing.

* Write modular modules.
   1. For each module, think that it can be used in another context than you originally intend to use it.
   2. For each module, think that it can be replaced by another module with the same interface.
   3. Write clear and simple interfaces for any module. A simple interface consists of a relatively small set of functions, that take very few parameters. UNIX system calls are a very good example of a very small set of function (about 100 functions), each takes very few parameters (1 or 2. Only one function takes 3 parameters). That set supplies all of the required functionality that an OS should supply.
   4. A module should not be aware of any module that is not logically connected to it. In other words: If a module X is not necessary for the definition of another module Y, than Y should not refer to X in any way.

# Code annotations

* EXCITEMENT platform will adopt some Java code annotations, starting with the followings.
   1. @LanguageDependent: Every class that is language dependent (e.g. only for English, only for Spanish, etc) should be annotated by @LanguageDependent.
   2. @ThreadSafe, @NotThreadSafe: These two annotations are for explicitly marking thread safety of a class.
   3. @ParserSpecific: Classes **and** methods which contain code that fits only a specific parser should be annotated with this annotation, where the value is the parse name. For example @ParserSpecific("easyfirst")
   4. @StandardSpecific: Classes **and** methods which contain code that fits only a specific standard (e.g., stanford-dependency standard of dependency parse tree relations) should be annotated with this annotation, where the value is the standard name. For example @StandardSpecific("stanford-dependencies")

* Actual definitions of the Java annotations and their usage will be provided by the implementation team
(WP4). Also, the list of the annotations will be expanded along with the implementation effort.