# Osprey coding standards

***This document is a work in progress.***

What follows below is a set of *recommended coding standards* for Osprey projects. These guidelines are divided into several sections – general [code formatting and appearance](#code-formatting-and-appearance), [naming conventions](#naming-conventions), [documentation conventions](#documentation-conventions), and [file structure](#file-structure).

In addition to the coding standards, this document describes a variety of [common patterns](#common-patterns) that authors are strongly encouraged to make use of, for consistency other modules.

## Table of contents

* [Code formatting and appearance](#code-formatting-and-appearance)
  - [Maximum line length](#maximum-line-length)
  - [White space](#white-space)
* [Naming conventions](#naming-conventions)
* [Documentation conventions](#documentation-conventions)
* [File structure](#file-structure)
* [Common patterns](#common-patterns)


# Code formatting and appearance

This tends to be the most controversial section by far, so let's just get it over with.

Above all, recognise that *code is read more often than it is written*. Keep it readable. If you return to your code and find that the formatting actively prevents you from understanding it, reformat your code.

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

Curly braces are used in three situations: to create hash tables, in member initialisers, and to delimit the block of a function member (including [lambda expressions](#lambda-expressions)). Curly braces for function members are generally placed on their own lines; see [Methods](#methods) and [Lambda expressions](#lambda-expressions). The rest of this section deals with hash creation expressions.

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
foo(
  (bar+baz**2)*quux   and  not
  unicorn
);
bird . chirp( new Volume(- 97), Frequency .high );

// Good:
foo(
  (bar + baz ** 2) * quux and
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


# Naming conventions

TODO


# Documentation conventions

TODO


# File structure

TODO


# Common patterns

TODO
