# Vyne-Language
Definition for the Vyne Programming language.

## Goals

* The Vyne language is a general purpose programming language.
* It should be cross-platform, or platform agnostic.
* Compile parameters should be included within the source code.

## Notes

The syntax shown in this document is tentative. Only used as a proof of concept.

## Features

### Choice

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

```
function(a, b) {
    var c;
    return a, b, c;
}
```

#### - Function type 3

Type 3 returns as a branching path. Each path can have it's own return signature. If the caller doesn't have a matching return branch, control percolates back through subroutine calls and/or nested blocks until a matching branch is found or until the end of the main program is reached, at which point the program is forcibly stopped with a suitable error message.

![](https://i.imgur.com/2vUwXgY.png)


#### - Function type 4

Type 4 is similar to a goto. Takes input parameters. Control flow is never returned.
