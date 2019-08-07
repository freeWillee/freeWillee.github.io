---
layout: post
title:      "Scoping it out"
date:       2019-08-07 11:02:10 -0400
permalink:  scoping_it_out
---


What is scope in JS? This is a really basic overview of what it is:

When we talk about scope, we mean where something is available. In the context of Javascript it is the set of rules that determine where a variable can be looked up and determined.

There are essentially two types of lookups when it comes to a variable: 1) for the purposes of assigning a variable; or 2) for the purposes of finding or retrieving the value of the variable.

Consider the following:
```

let a = 2
```

The Javascript engine goes through two phases when the program is run – the compilation phase and the execution phase. In the compilation phase, the let a portion of the code above is compiled and space in memory is set aside for the variable a which has yet to be assigned a value. Further, this variable declaration is said to be declared in the currently executing scope, or its execution context. If we are not given any other information and just see the line of code above, we can infer that this line of code, in the compilation phase, the variable a is declared in the global scope.

In the next phase, the execution phase, the variable a is assigned the value 2. Because a is declared in the global context, the variable a is available anywhere in the program.

For example:

```
let a = 2

function compute(b) {
let c = 2 
console.log(a + b + c)
}

compute(2) // => 6
```

There are few things happening here: firstly a is declared in the global scope, the function compute is declared in the global scope, and the variable c is declared in the nested scope of the function. b is also implicitly declared in the function definition as well. When function above is invoked, the engine knows to run the function compute and runs into the first variable c, which in is the value 2, declared in the nested scope (not global). The next line of code logs to the console the sum of 3 variable: a, b, and c. For a, the engine first looks in the current execution context of the function compute for the definition of a. Not finding this, it then goes up a level to the global scope to find that a is equal to 2. Next is b: it is defined in the function as being the value that is passed in as a parameter to the function; in this case, it is 2. Finally, c: this is defined in the execution context of the function just one line up, and is the value 2. Armed with the values for all three variables, we can see that 6 is logged to the console.

If we check in the console again what the value of a is, we will see 2. If we try to check the value of the other variables b or c, we will see a ReferenceError. This is because when we are checking the values of the variables now, we are in the global scope. Since b and c were declared in the nested scope of the function, they are not visible to the global scope!

The takeaway here is that all variables (and functions) declared in the global scope can be assessed anywhere in the program. But it doesn’t work the other way around — any variables are declared in a nested scope, will be limited to anywhere within that scope only.

Understanding scope helps to protect and isolate your variables to only be used in places where it is supposed to be used; this can help in maintaining readability as well. Consider the following case where you do not want the internal rate of return to be manipulated in anyway outside of the following function because this function is a crucial piece of code that the rest of the program and results rely heavily on:

```
function getInterestPayment(principal) {
let interestRate = 0.08;
return interestRate * principal
}
```

If you run this code, getInterestPayment(50), you would get 4. If another team member decides to use the variable interestRate outside of this program, they would have to declare it again and it would not impact the output of this function. This is because the interestRate variable is protected within the scope of the function declaration above.

This leads into another topic on ‘closures’ which I will write about in another post. Until next time, happy coding! goes here.
