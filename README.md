# Vyne-Language
Definition for the Vyne Programming language. The Vyne language is an imaginary programming language. There are no known compilers that exist yet.

## Index
* [Goals](#goals)
* [Far Fetched Goals](#far-fetched-goals)
* [Notes](#notes)
* [Features](#features)
  * [Comments](#comments)
  * [Choices](#choices)
    * [if](#small_orange_diamond-if)
    * [switch](#small_orange_diamond-switch)
  * [Loops](#loops)
    * [while](#small_orange_diamond-while)
    * [for](#small_orange_diamond-for)
    * [do while](#small_orange_diamond-do-while)
    * [loop](#small_orange_diamond-loop)
    * [foreach](#small_orange_diamond-foreach)
  * [Subroutines](#subroutines)
    * [Function type 1](#small_orange_diamond-function-type-1)
    * [Function type 2](#small_orange_diamond-function-type-2)
    * [Function type 3](#small_orange_diamond-function-type-3)
    * [Function type 4](#small_orange_diamond-function-type-4)
  * [Boolean operators](#boolean-operators)
  * [Scope](#scope)

## Goals

* The Vyne language is a general purpose programming language.
* It should be cross-platform, or platform agnostic.
* Compile parameters should be included within the source code.

## Far Fetched Goals

* Syntax agnostic. For example a simple loop has a condition and a code block. Syntax files could be written to define how to parse code. Within a file, parser information could be provided somehow. This would make it possible for a single project to have many files written with different syntax. Under all the syntax, you'd still have pure Vyne.
* Ability to execute compiled and non compiled code side by side. This would make it possible to keep parts of a project uncompiled allowing runtime code editing.

## Notes

The syntax shown in this document is tentative. Only used as a proof of concept.

## Features

### Comments

Support for single line comments and nested multiline comments.

The current syntax for multiline comments:

```
/*
    This is inside the comment
    /*
        You can insert something here.
    */
    This is a comment since the nested comment is parsed correctly.
*/
```

There is also a way to break out of nested comments:

```
/*
    /*
        /*
            Comment here
*//
```

### Choices

#### :small_orange_diamond: If

Works like other languages.

#### :small_orange_diamond: Switch

Works like other languages. Will be closer to functional languages with pattern matching.

### Loops

#### :small_orange_diamond: While

The while loop has extra features compared to other languages.

```csharp
while (condition) {

}
else while (condition) {

}
else loop {

}
```

The Vyne while loop works like an if statement.

It starts by checking the first condition. If it is true, it will enter that branch until the condition becomes false.

If the first condition was false, it will check the second condition. If it is true, it will enter that branch until the condition becomes false.

If the second condition was also false, it will execute the final else loop. The else loop here is an infinite loop that requires manual breaking out.

This new while loop is now a special case of the if statement. It makes it possible to have this syntax:

```csharp
if (condition) {

}
else while (condition) {

}
else if (condition) {

}
else {

}
```

#### :small_orange_diamond: For

Works like other languages.

#### :small_orange_diamond: Do While

Works like other languages.

#### :small_orange_diamond: Loop

An infinite loop that requires manual breaking out.

```csharp
loop {

}
```

#### :small_orange_diamond: Foreach

Most likely will work like other languages.

### Subroutines

#### :small_orange_diamond: Function type 1

Type 1 works similar to functions in other languages. Takes input parameters and returns output parameters. Control flow is returned back to the caller.

#### :small_orange_diamond: Function type 2

Type 2 doesn't have the ability to produce side effects. Takes read-only input parameters and returns write-only output parameters. If the same variable is passed as an input and output, then some optimizations can be applied. For example a variable could end up being passed as a reference, or it could be passed by value with deep copy. Control flow is returned back to the caller.

For example, the following function takes 2 input variables and returns 3 output variables:

```csharp
var a;
var b;

var c, d, e = function(a, b);

function(f, g) {
    f += 1;
    g += 1;
    var h;
    return f, g, h;
}
```

The original variables `a` and `b` are not modified. They are passed by value.

The variables `c`, `d`, `e` are write-only from the function's point of view.

```csharp
var a;
var b;

a, b = function(a, b);

function(a, b) {
    a += 1;
    b += 1;
    return a, b;
}
```

In the example above, the caller gives explicit permission to the function to modify `a` and `b`. As such they are passed by reference.

```csharp
var a;
var b;

a, b = function(a, b);

function(a, b) {
    return b, a;
}
```

This last one could be used to swap variables.

A type 2 function can only call other type 2 functions to preserve the no side effect property.

#### :small_orange_diamond: Function type 3

Type 3 returns as a branching path. Each path can have it's own return signature. If the caller doesn't have a matching return branch, control percolates back through subroutine calls and/or nested blocks until a matching branch is found or until the end of the main program is reached, at which point the program is forcibly stopped with a suitable error message.

![](https://i.imgur.com/2vUwXgY.png)

#### :small_orange_diamond: Function type 4

Type 4 is similar to a goto. Takes input parameters. Control flow is never returned.

### Boolean operators

Currently proposed boolean operators:

```
==
<
>
!=
<=
>=
!<
!>
```

`!<` and `!>` are equivalent to `>=` and `<=`. In some cases, it is useful to represent logic using one or the other to make an algorithm's purpose clearer.

Boolean operators have syntactic sugar to make it easier to write common logic using `&` and `|`:

```
0 < i &< 10
becomes
0 < i && i < 10
```
```
0 < i |< 10
becomes
0 < i || i < 10
```

### Scope

The concept of a scope is very important in the Vyne language. Where does something exist? Where something lives needs to always be explicit. A global variable would only be a variable that is made explicitly accessible within other scopes.
