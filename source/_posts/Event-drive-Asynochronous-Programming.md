---
title: Event-drive Asynochronous Programming
tags:
  - Node
  - JS
  - Asyn
categories:
  - Event-drive Asynochronous Programming
comments: true
mathjax: true
abbrlink: '2e74'
date: 2018-10-07 00:16:03
---
<p></p>
<!--more-->

# Syntax

## Declaration: var\const\let
Only use let except you know it would never change.
A variable declared with `var` whose scope is within the function in which the statement is located, and there is a variable promotion phenomenon;
A variable declared with `let` whose scope is within the code block in which the statement is located, and there is no variable promotion;
The use of const declares a `const`, and the value of the constant cannot be modified in the code that appears later.

## === c.f. ==
Always use=== unless you have specially reason to use ==, it. 
=== will check both value and type.

>[from Zhihu@Belleve][1]
> Red：=== 
> Orange：== 
> Yellow：<= and >= established，== not established 
> Blue：only >= established
> Green：only <= established
>![===and== from Zhihu@Belleve][2]

## Template String ${var}
```JS
var firstName = 'Jake';
var lastName = 'Rawr';
//normal String
console.log('My name is ' + firstName + ' ' + lastName);
//Template String
console.log(`My name is ${firstName} ${lastName}`);
```
## Fuctions and arrow funcitons
```JS
fuction inc1=1(n){
return n + 1
}
const inc2=function(n){return n+1}
const inc3=(n)=>{return n+1}
const inc4=n=>n+4
//they are all same.

fucntion adder(inc) {
return (n)=>n+inc
}
const addOne=adder(1)
const addTwo=adder(2)
let a=addOne(5)//a=6
let b=addTwo(5)//b=7
let c=adder(2)(1)//c=3 
//use pointers to point the functions' addres .
```
## JavaScript Scope （作用域）: Fuction-level
In JS there are two types of scope: Local scope and Global scope
**JavaScript has function scope: Each function creates a new scope.** The traditional C-like language, whose scope is block-level scope, curly brackets`{}` are a scope. But for JavaScript, its scope is a function-level scope, such as an `if` conditional statement, which is not an independent scope:
```JS
var x = 1;
console.log(x); // 1
if (true) {
var x = 2;
console.log(x); // 2
}
console.log(x); // 2

//Create a temporary scope from a self-executing function
function foo() {
var x = 1;
if (x) {
(function () {
var x = 2;
// some other code
}());
}
// x is still 1.
}

```
## Event-drive and Asynchronous

There is only one main thread in JS engine. When it is executing JS code block, other code block are not allowed to be executed, but the code block of event machine and callback will be added into task queue (or called stack).This event or callback function will be executed when a trigger meet the event or callback.

### Callbacks
callbacks are historically the only way to deal with functions that return a result asynchronously.

### [Promise][3]


A Promise is in one of these states:

- pending: initial state, neither fulfilled nor rejected.
- fulfilled: meaning that the operation completed successfully.
- rejected: meaning that the operation failed.
![Promise state][4]

[1]: https://www.zhihu.com/question/31442029
[2]: https://pic3.zhimg.com/80/b922270259dece707ef6c6a50259a406_hd.png
[3]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[4]: https://mdn.mozillademos.org/files/15911/promises.png
