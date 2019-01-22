# Vyne-Language
Definition for the Vyne Programming language.

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

### Choices

#### - If

Works like other languages.

#### - Switch

Works like other languages. Will be closer to functional languages with pattern matching.

### Loops

#### - While

The while loop has extra features compared to other languages.

```csharp
while (condition) {

}
else while (condition) {

}
else {

}
```

The Vyne while loop works like an if statement.

It starts by checking the first condition. If it is true, it will enter that branch until the condition becomes false.

If the first condition was false, it will check the second condition. If it is true, it will enter that branch until the condition becomes false.

If the second condition was also false, it will execute the else. The else here is an infinite loop that requires manual breaking out.

#### - For

Works like other languages.

#### - Do While

Works like other languages.

#### - Loop

An infinite loop that requires manual breaking out.

```csharp
loop {

}
```

#### - Foreach

Most likely will work like other languages.

### Subroutines

#### - Function type 1

Type 1 works similar to functions in other languages. Takes input parameters and returns output parameters. Control flow is returned back to the caller.

#### - Function type 2

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

a, b = function(a, b) {
    a += 1;
    b += 1;
    return a, b;
}
```

In this last example, the caller gives permission to the function to modify `a` and `b`. As such they are passed by reference.

A type 2 function can only call other type 2 functions to preserve the no side effect property.

#### - Function type 3

Type 3 returns as a branching path. Each path can have it's own return signature. If the caller doesn't have a matching return branch, control percolates back through subroutine calls and/or nested blocks until a matching branch is found or until the end of the main program is reached, at which point the program is forcibly stopped with a suitable error message.

![](https://i.imgur.com/2vUwXgY.png)

#### - Function type 4

Type 4 is similar to a goto. Takes input parameters. Control flow is never returned.
