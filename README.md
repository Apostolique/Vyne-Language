# Vyne-Language
Definition for the Vyne Programming language. The Vyne language is an imaginary programming language. There are no known compilers that exist yet.

## Discord

[![Discord](https://img.shields.io/discord/530598289813536771.svg)](https://discord.gg/dHkA4bP)

## Index
* [Goals](#goals)
* [Far Fetched Goals](#far-fetched-goals)
* [Notes](#notes)
* [Features](#features)
  * [Comments](#comments)
  * [Blocks](#blocks)
    * [Basic](#small_orange_diamond-basic)
    * [Deferred](#small_orange_diamond-deferred)
    * [Paralleled](#small_orange_diamond-paralleled)
  * [Block Chaining](#block-chaining)
    * [Else](#small_orange_diamond-else)
    * [Then](#small_orange_diamond-then)
  * [Choices](#choices)
    * [If](#small_orange_diamond-if)
    * [Switch](#small_orange_diamond-switch)
  * [Loops](#loops)
    * [While](#small_orange_diamond-while)
    * [For](#small_orange_diamond-for)
    * [Loop](#small_orange_diamond-loop)
    * [Do while](#small_orange_diamond-do-while)
    * [Foreach](#small_orange_diamond-foreach)
  * [General Statements](#general-statements)
    * [Delay expression](#small_orange_diamond-delay-expression)
    * [Label](#small_orange_diamond-label)
    * [Break](#small_orange_diamond-break)
    * [Continue](#small_orange_diamond-continue)
  * [Subroutines](#subroutines)
    * [Function type 1](#small_orange_diamond-function-type-1)
    * [Function type 2](#small_orange_diamond-function-type-2)
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

Loose multiline comment terminators are ignored as whitespace:

```
*/*/*/
```

### Blocks

There are 3 different types of code blocks.

#### :small_orange_diamond: Basic

Starts a local scope. The scope is cleaned up when the end of the scope is reached.

```
{

}
```

#### :small_orange_diamond: Deferred

Starts a local scope. The scope is cleaned up when the parent scope is cleaned up.

```
{+>

}
```

#### :small_orange_diamond: Paralleled

Doesn't start a new scope. Memory is cleaned up when the current scope is cleaned up. This block is brought to the top of the current scope to be executed first either sequencially or in parallel with the other parallel blocks in the scope.

```
{|>

}
```

Can be used like this:

```lisp
{
   let c = a + b;

   {|>
      let a = 0;
   }
   {|>
      let b = 10;
   }

   {
      let e = a + d;

      {|>
         let d = 20 + c;
      }
   }
}
```

### Block Chaining

Blocks can be chained using the `else` and `then` keywords.

#### :small_orange_diamond: Else

The `else` keyword is used to execute a block when the first block was not executed.

```
{
    //This gets executed.
}
else {
    //This never gets executed.
}
```

#### :small_orange_diamond: Then

The `then` keyword is used to always execute a block when the first block was executed.

```
{
    //This gets executed.
}
then {
    //This gets executed.
}
```

### Choices

#### :small_orange_diamond: If

```csharp
if condition {

}
else if condition {

}
else {

}
```

#### :small_orange_diamond: Switch

Works like other languages. Will be closer to functional languages with pattern matching.

### Loops

#### :small_orange_diamond: While

The `while` loop has extra features compared to other languages.

```csharp
while condition {

}
else while condition {

}
else loop {

}
```

The Vyne while loop works like an `if` statement.

It starts by checking the first condition. If it is true, it will enter that branch until the condition becomes false.

If the first condition was false, it will check the second condition. If it is true, it will enter that branch until the condition becomes false.

If the second condition was also false, it will execute the final else loop. The else loop here is an infinite loop that requires manual breaking out.

This new `while` loop can be mixed with other statements such as the `if` statement. It makes it possible to have this syntax:

```csharp
if condition {

}
else while condition {

}
else if condition {

}
else {

}
```

#### :small_orange_diamond: For

Works like other languages.

#### :small_orange_diamond: Loop

An infinite loop that requires manual breaking out.

```csharp
loop {

}
```

#### :small_orange_diamond: Do While

Can be done using `loop`.

```csharp
loop {
    //Some code here.
    
    if condition {
        break;
    }
}
```

#### :small_orange_diamond: Foreach

Most likely will work like other languages.

### General Statements

#### :small_orange_diamond: Delay Expression

The delay expression is used to delay the execution of a block. It can be used to create code comments:

```csharp
...{
    // Some code.
    // It will never be executed.
}
```

It is also possible to catch the definition in a variable to execute it later:

```csharp
let Point = ...{+>
    let X = 10;
    let Y = 20;
};

let a = Point!;
let b = Point!;

a.X = 15;
```

This can be used to define reusable code.

#### :small_orange_diamond: Label

It is possible to add labels to some statements.

```csharp
while :outer condition {
    while :inner condition {
    
    }
}
```

#### :small_orange_diamond: Break

A break is used to exit out of a loop.

```csharp
loop {
    break;
}
//We end up here after the break.
```

In nested loops, it is possible to specify which loop to break out of using labels.

```csharp
while :outer condition {
    while :middle condition {
        while :inner condition {
            break middle;
        }
    }
    //We end up here after the break.
}
```

#### :small_orange_diamond: Continue

A continue is used to skip to the end of a loop iteration.

```csharp
while condition {
    continue;
    
    //Some code that is never reached.
    
    //We end up here after the continue.
}
```

The continue can also be used with labels.

```csharp
while :outer condition {
    while :middle condition {
        while :inner condition {
            continue middle;
        }
        //We end up here after the continue.
    }
}
```

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

### Boolean operators

Currently proposed boolean operators:

```
==
!=
<
>
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

The concept of a scope is very important in the Vyne language. Where does something exist? Where something lives needs to always be explicit. A global variable would only be a variable that is made explicitly accessible within other scopes. It is possible to name scopes and pass them as function parameters.

#### Scope dependency

It is possible to define a scope as dependent on external factors. This makes it possible for a scope to access variables that are external to itself. It's up to the parent scope to satisfy those dependencies.
