# Osprey coding standards

***This document is a work in progress.***

What follows below is a set of *recommended coding standards* for Osprey projects. These guidelines are divided into several sections – general [code formatting and appearance](#code-formatting-and-appearance), [naming conventions](#naming-conventions), [documentation conventions](#documentation-conventions), and [file structure](#file-structure).

In addition to the coding standards, this document describes a variety of [common patterns](#common-patterns) that authors are strongly encouraged to make use of, for consistency other modules.

## Table of contents

* [Code formatting and appearance](#code-formatting-and-appearance)
  - [Maximum line length](#maximum-line-length)
  - [White space](#white-space)
* [Naming conventions](#naming-conventions)
  - [Capitalization](#capitalization)
  - [Abbreviations](#abbreviations)
  - [Namespace names](#namespace-names)
  - [Type names](#type-names)
  - [Method names](#method-names)
  - [Property and field names](#property-and-field-names)
  - [Constant names](#constant-names)
  - [Keyword escapes](#keyword-escapes)
* [Documentation conventions](#documentation-conventions)
* [File structure](#file-structure)
* [Common patterns](#common-patterns)


# Code formatting and appearance

This tends to be the most controversial section by far, so let's just get it over with.

Above all, recognize that *code is read more often than it is written*. Keep it readable. If you return to your code and find that the formatting actively prevents you from understanding it, reformat your code.

## Maximum line length

**Soft limit: 120 characters.** Endeavour to keep lines below 80 characters.

> 80 characters is seen as too restrictive in an age of significant wide-screen monitor adoption. But many developers still wish to view multiple code windows side-by-side, so keep your lines short.

Long error messages in `throw` statements may cause the line to exceed 120 characters; that's fine. (Do still try to keep error messages short and to the point.)

## White space

### Indentation

**Indent with tabs, align with spaces.**

> Indenting with tabs lets everyone pick their preferred indentation size; aligning with spaces ensures code looks consistent for everyone (assuming a monospace font is in use).

*(Code examples in this document are indented with 2 spaces because most browsers default to 8 spaces for a tab, which is rather unsightly. Feel free to consider this an argument in favour of spaces.)*

Example (using `→` to represent a tab):

```
public class T
{
→public new();

→public get something = ...;

→override toString()
→{
→→return ...;
→}

→private static usefulData = {
→→"foo":     fooValue,
→→"bar":     barValue,
→→"baz":     bazValue,
→→"quux":    quuxValue,
→→"cthulhu": unspeakable,
→}
}
```

### Alignment

**Align sparingly.** If you must align, do so with spaces, never tabs.

> Alignment is useful in situations where the data is highly unlikely to change much over time, and is most frequently used to align keys and value in hash tables. See the example under [Indentation](#indentation). Downsides include added maintenance cost and unnecessarily large diffs.

There are some situations where it's *never* acceptable to align:

* Values in local variable declarations;
* Initializers from different field declarations;
* Enum values;
* Default parameter values (if the parameter list is broken up over multiple lines);
* Comments at the end of lines (put the comment above instead); and
* Across different levels of indentation (tabs can vary in size).

In addition, alignment is done *only* on consecutive lines. If there is anything inbetween, even a blank line, do not use alignment. Be aware that overuse of alignment can result in needlessly large diffs. If in doubt, don't do it.

```
// Bad:
function f(
  longParameterName     = someDefaultValue,
  anotherOverlyLongName = moreDefaults
)
{
  var gee   = ...;
  var aitch = ...;
  var i     = ...;
}

enum E
{
  one   = 1,
  two   = 2,
  three = 3,
  four  = 4,
}

// Good:
function f(
  longParameterName = someDefaultValue,
  anotherOverlyLongName = moreDefaults
)
{
  var gee = ...;
  var aitch = ...;
  var i = ...;
}

enum E
{
  one = 1,
  two = 2,
  three = 3,
  four = 4,
}
```

### Parentheses

**No space after `(` or before `)`.** This includes parenthesized expressions – `a * (b + c)` – as well as parameter and argument lists – `f(a, b, c)`.

**Space before `(` or after `)` *only* when dictated by other rules.** E.g.: space around infix operators; space after `if`, `for`, `return`, etc. See other sections.

```
// Bad:
function foo( bar, quux )
{
  return( bar >> bar ) ( quux );
}

// Good:
function foo(bar, quux)
{
  return (bar >> bar)(quux);
}

// Bad:
function foo (a, b, c)
{
  return a (b, c (a)) ;
}

// Good:
function foo(a, b, c)
{
  return a(b, c(a));
}
```

### Square brackets

Square brackets are used in two situations: to create lists, and to access or define indexers. In the case of indexers, you must also follow the rules for [argument lists](#argument-lists) and [parameter lists](#parameter-lists).

**No space after `[` or before `]`.**

**Space before `[` or after `]` *only* when dictated by other rules.** E.g.: space around infix operators; space after `if`, `for`, `return`, etc. See other sections.

```
// Bad:
return[ 1, 2, 3 ];

// Good:
return [1, 2, 3];

// Bad:
list [ i ] = list [ i-1 ];

// Good:
list[i] = list[i - 1];
```

### Curly braces

Curly braces are used in three situations: to create hash tables, in member initializers, and to delimit the block of a function member (including [lambda expressions](#lambda-expressions)). Curly braces for function members are generally placed on their own lines; see [Methods](#methods) and [Lambda expressions](#lambda-expressions). The rest of this section deals with hash creation expressions.

**No space after `{` or before `}`.**

**Space before `{` or after `}` *only* when dictated by other rules.** E.g.: space around infix operators; space after `if`, `for`, `return`, etc. See other sections.

*But note:* single-line hash creation expressions are rarely a good idea. They tend to become a confusing mess of colons and commas. Think twice. (Do not use hashes as "light-weight" objects as if you were writing JavaScript.)

```
// Bad:
var data={ "a": a, "b": b };
var thing = new Thing with{ value: 4 };

// Good:
var data = {"a": a, "b": b};
var thing = new Thing with {value: 4};
```

### Operators

**White space before and after infix operators.** This applies to *all* infix operators, including `::`, `->` and `**`. Note that member access (`.`) is not an infix operator.

**One space after `not`, no space before.** Even if the thing following `not` begins with punctuation. **No space before or after any other unary operators.** When preceded by an infix operator, the rule about spacing infix operators still applies. Hence, `a + ~b`, not `a +~b`, because infix `+` must have a space after it. But do write `a(~b)`, not `a( ~b)`.

When broken onto multiple lines, **infix operators go at the end** of the first line and subsequent lines are indented the same as the first line, unless stated otherwise.

See also [rules for conditional expressions](#conditional-expressions) (`?:`).

```
// Bad:
bird=foo(
  quux*(bar+baz**2)and not
  unicorn
);
bird . chirp( new Volume(- 97), Frequency .high );

// Good:
bird = foo(
  quux * (bar + baz ** 2) and
  not unicorn
);
bird.chirp(new Volume(-97), Frequency.high);
```

### Blank lines

**Blank lines are *blank*.** You may not leave trailing white space on the line. Configure your editor to strip it.

> If trailing white space on blank lines were permitted but not required, inconsistencies would arise. If required, it would be a nightmare to maintain. Since all widely-used editors can be configured to strip trailing white space on save, this is the least painful alternative.

Blank lines should be used sparingly, but are *required* under some circumstances:

* Between definitions of types, methods (including invocators), properties (including indexers), enum fields, operator overloads and iterators.
* Between *public and protected* fields. Private fields may (but are not required to) be grouped together.
* To separate logical sections in a method. But beware: if your method has a great number of sections, it probably does too many things.

Do not use multiple consecutive blank lines.

**All files end with exactly one newline character,** which your editor may display as a blank line.

### Trailing spaces

**No.** Configure your editor to strip them. No exceptions.

***(TODO)***


# Naming conventions

**Above all, strive for clarity.** The name of your member should be self-explanatory. You can generally assume your user is familiar with domain-specific terminology (but it doesn't hurt to describe more complex subjects in your documentation).

**Use American English spelling.** "Color", not "colour"; "initialize", not "initialise"; "meter", not "metre".

> Using one single consistent spelling makes it easier for everyone.

**Be dull and uncreative.** Don't try to be cute or funny with your names: `getWatcher()` is better than `watchify()`; `makeBold()` says more than `embolden()`.

## Capitalization

**Type names use `UpperCamelCase`. Everything else uses `lowerCamelCase`.** Do not use `UPPER_SNAKE_CASE` for constants. Avoid underscores, except as noted below. Remember that Osprey is a case-sensitive language; be consistent with your capitalization and underscore usage.

If you must use [abbreviations](#abbreviations), follow these two rules:

* If the abbreviation consists of two characters, give them the same capitalization, according to the abbreviation's position in the name: `IOError` (type name), `ioError` (non-type name), `currentOSVersion`.
* Otherwise, treat it as a regular word. That means capitalize the fist letter only, if it should be capitalized: `HtmlDocument` (type name), `cpuCores` (non-type name), `availableRam` (better: `availableMemory`).

```
// Bad:
namespace Acme.Camel;

public class dataContainer
{
  public get Important_value = 42;

  public calculate_sum()
  {
    var CurrentSum = 0;
    ...
  }

  private const MAX_USEFULNESS = 6;
}

// Good:
namespace acme.camel;

public class DataContainer
{
  public get importantValue = 42;

  public calculateSum()
  {
    var currentSum = 0;
    ...
  }

  private const maxUsefulness = 6;
}
```

## Abbreviations

**Abbreviate only when the meaning is clear,** and even then, do it with caution.

Some abbreviations are known or are expected to be learned by everyone – such as I/O, OS, GC, RAM, CPU, WWW, and so on. These are usually fine to use. Sometimes it may be better to substitute alternative words – "memory" instead of "RAM", "processor" instead of "CPU".

Domain-specific abbreviations must be evaluated on a case-by-case basis. Some guidelines:

* If the abbreviation is widely used and understood within the domain, it is probably OK to use it: `HttpServer` is preferable to `HypertextTransferProtocolServer`.
* If the expanded phrase is particularly cumbersome, it may be OK to abbreviate it. In a module all about virtual reality integration, `VRHeadset` is better than `VirtualRealityHeadset`.

As a general rule, prefer *not* abbreviating. In particular, **do not abbreviate if it reduces clarity**.

## Namespace names

**Namespace names should generally be single nouns,** but if you need multiple words, use `lowerCamelCase`, not underscores. Prefer nouns. If your namespace contains many members with similar purpose or functionality, consider a plural noun: `util.collections`.

**Use namespaces responsibly.** Not everything needs its own namespace. Small modules can often get away with putting everything in a single namespace. Avoid excessively deep nesting.

**Prefer two or more components,** ideally structured as `<company>.<product>`. If the company and product name are identical, omit one of them. Sometimes `<product>.<subproduct>` is acceptable, e.g. `lua.compiler`. If there is no company name, use a generic category name: `io`, `testing`, `net`, `math`, `graphics`, etc.

Namespaces may be named after commercial products. If the product name uses InterCaps, it is generally best to lowercase the whole name, unless it happens to follow Osprey style already: `google.youtube`; but `apple.iOS` or `apple.ios` (at your discretion). If there is a precedent, follow it.

```
// Bad:
namespace Corel.WordPerfect { }
namespace windowsPhone { }
namespace company.product.feature.errors.internal.fatal { }

// Good:
namespace corel.wordperfect { }
namespace microsoft.windows.phone { }
namespace company.product.feature { }
```

## Type names

**Type names are nouns,** or sometimes (rarely) adjectives. The type name succinctly summarizes the type's purpose and functionality. Agent nouns (ones ending in *-er*) are common. Adjectives and other nouns can modify the type name as needed.

Adjective names should be avoided, but are sometimes used, especially for abstract base classes that describe a capability. An example is `Iterable`.

**Use specific names.** Avoid overly general terms, except if *very* well established: `Socket` is OK, but `Node` is too vague (prefer `XmlNode` or `ParseNode` or `RedBlackTreeNode`).

**Do not rely on the containing namespace as a sufficient qualifier of your type name.** It is common practice to import all members from a namespace. Your type name should be understandable without namespace qualifier. This also makes it easier to use your type together with similarly-named types from other modules.

Enum sets primarily use plural nouns, since values of such types represent the combination of multiple options.

```
// Bad:
public class Parse { }
public class ThrowErrors { }
public enum set FlagsForTheEncoder { }

// Good:
public class Parser { }
public class ErrorHelpers { }
public enum set EncoderFlags { }

// Better:
public class PythonParser { }
```

## Method names

**Methods use verbs or verb phrases.** The name of the method succinctly describes what the method does. If you find yourself using "and" in the method name, it should probably be two methods.

Sometimes verbs can be elided. If there is a strong convention and the meaning is clear enough, you might get away with omitting the verb. Example: `MyType.createFromString()` would more conventionally be `MyType.fromString()` (or perhaps even better, `MyType.parse()`).

It may be unclear whether a method acts on itself or takes an argument to do things to. Since unnecessary state should be avoided, most methods will act on their arguments. In the case of ambiguity, make it clear that the method expects an argument: `saveTo(fileName)` instead of `save(fileName)`; `writeText(value)` instead of `write(value)`. Do this within reason.

Do not use methods to obtain read-only values. Declare a read-only property instead.

If several public methods call through to a common internal method, name it after the public method plus the suffix `Internal` or `Inner` (at your discretion). Do not use an underscore prefix.

```
// Bad:
public to(type) { }
public name() { }
public serializeAndWriteToStream(stream) { }

// Good:
public convertTo(type) { }
public getName() { }
public writeSerialized(stream) { }

// Better:
public get name { }
```

## Property and field names

**Properties and fields use nouns or noun phrases.** The name should typically be able to fill in the blank in "this is the object/type's \_\_\_\_" or "these are the object/type's \_\_\_\_".

The size of a collection is called `length`. (Also consider inheriting from `aves.Collection` whenever possible.)

If the member contains the number of something, call it `somethingCount`, not alternatives like `somethingLength`, `somethingNumber` or `numOfSomething`. But note: on its own, `count` is treated as a verb. Also note: `obj.children.length` is preferable to `obj.childCount`; expose collections when you can.

Backing fields for properties are named after the property, with a leading underscore. Other fields, even when private, should not be underscore-prefixed.

```
// Bad:
public get number_of_nodes { }
private __superSecretValue__;

private val;
public get value = val;

// Good:
public get nodeCount { }
private superSecretValue;

private _value;
public get value = _value;
```

## Constant names

Follow the rules for [field names](#property-and-field-names). Use `lowerCamelCase`, not `UPPER_SNAKE_CASE`.

## Keyword escapes

**Use keyword escapes only when necessary.** Usually it is clearer to use a synonym:

* `type` instead of `\class`;
* `target` instead of `\for`;
* `elseClause` instead of just `\else`.

But the feature exists, so don't be afraid to use it if you actually have to.


# Documentation conventions

**Document public and protected members,** and ideally everything else too. Documentation exists to help you and your users alike. Osprey has documentation comments for a reason. *Use them.*

**Always strive for clarity.** Conciseness at the cost of clarity is worthless; verbose word salad equally so. A perfectly technically accurate phrasing that almost no one understands must be rewritten. A ten-page essay that says nothing must be significantly shortened. The purpose of documentation is to *communicate and inform*. If your documentation fails to do that, rework it.

**Speak in positive, direct and honest terms:**

* Describe what the member *does*, not what it *doesn't* do.
* Avoid excessive formalities and verbosity. Say what is necessary; no more, no less.
* Do not suggest emotion or intention where there is neither. A network error doesn't occur because "The OS felt like closing the socket"; it occurs because "The socket was closed by the OS".
* Do not act apologetic (or insulting) about possible error conditions. A method throws an `ArgumentNullError` when "`value` is null", not because "You were stupid enough to pass null into `value`", nor because "Sorry, `value` can't be null :(".

A documentation comment is made up of several sections, always in this order:

* [Summary](#summary-section) – brief description
* [Param x](#param-section) – one for each parameter
* [Returns](#returns-section) – return value
* [Throws x](#throws-section) – one for each error thrown by the method
* [Remarks](#remarks-section) – detailed information

The *Param*, *Returns* and *Throws* sections are only applicable to function members.

## Summary section

**A brief description of the member.** A single sentence (or, in the case of methods, sentence fragment) that describes what the member is or does. Avoid code references in the summary. End the summary with a full stop.

A well-written summary answers the question "Is this what I'm looking for?". For function members, it also informs the user (roughly) what kind of input is expected.

While a summary should be brief, it is occasionally acceptable to have more than one sentence, when it is necessary to impart additional information without wishing to send the user to the remarks section. For example:

* `aves.Hash` also states: "A key can be any arbitrary value except null."
* Many of the methods in `io.Stream` say things like: "The method blocks until the data has been written."

For function members, the summary should fill in the sentence "This method/constructor/operator \_\_\_\_". Do not use the imperative, as in "Calculate the frobniz". Do not use negated verb phrases like "Does not ...". Always write about what the function member *does*, not what it *doesn't* do. Note that verbs like "skips", "hides", "closes", "destroys" and similar are not negated; use them when appropriate.

Avoid saying that a function member "Returns *xyz*", or worse, "Returns the result of *verb*ing ...". Use more specific phrasings instead: "Calculates the foo of the first bar", "Reduces the zorp to a florq". It is assumed that a function member returns the result of its calculation.

To describe the inputs to a function member, do not refer to parameter names. The summary should be understandable without looking at the function member's signature. Instead, use the phrasing "the specified ...". Example: "Calculates the absolute value of the specified number".

To refer to the current instance, say "the current instance/sequence/list/hash/...", or simply "this instance/sequence/list/hash/...".

If you employ technical language in the summary, write a [remarks section](#remarks-section) that rephrases the summary in plainer language, and preferably give code examples. See [Remarks section](#remarks-section) for more details.

Some examples:

* "Represents an error that occurs when an I/O operation fails." (`io.IOError`)
* "Creates a new {Parser} with the specified source file, flags and error manager." (`osprey.compiler.Parser.new`)
* "Writes the entire byte contents of this stream into the specified destination stream." (`io.Stream.copyTo`)

### Standard summary phrasings

There are certain standard phrasings that are *strongly* recommended:

* Error types: start with "Represents an error that occurs when ...".
* Constructors: "Creates a new {Type} with ..." (where `Type` is always the name of the containing type).
* Function members returning a boolean: "Determines whether ...".
* Property and indexer accessors: "Gets ..." for getters, "Sets ..." for setters. Do not use verbs like "Fetches", "Loads", "Stores", "Saves", or any other alternatives.
* *(TODO)*

## Param section

**Documentation of a parameter,** including its intended use and accepted types.

TODO


# File structure

TODO


# Common patterns

TODO
