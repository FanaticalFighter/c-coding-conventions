# C Coding Convention

This is my personal convention for writing C code. I decided to write this after being frustrated with everything else that I've seen out there. This is designed to be easy to read, and easy to write.

I used the Linux kernel coding style guide as a model for this. However, there are massive differences between the two.

###  Indentation

Indentation is done with tabs. Why tabs? Because tabs are good for indentation is why. With spaces, I have to either configure my editor to replace my typing tab with spaces or (_even worse_) type out the spaces.

Tabs are 4 characters. Having 8 character indentation makes things go out of hand really, really fast, and having 2 character indentation makes making out different levels of indentation hard. Of course, professional programmers don't use numbers that aren't multiples of 2, so other, closer numbers are out of the question.

Different logical units should be split into their own paragraphs, seperated by a blank line. This makes reading code much easier because you can understand what each chunk of code does much more easily.

As a general rule, every scope is indented by one tab. So, the bodies of loops, conditionals, functions, etc. all are indented. The one exception to this are cases. Cases stay on the same line as the parent switch.

```c
int i;
for (i = 0; i < 10; i++)
{
	printf("%d", i);
}

if (i > 10) 
{
	printf("How did I get here?\n");    
}

switch (i)
{
case 1:
case 2:
    break;
case 10:
	printf("This should happen... I think.\n");
}
```

And no trailing whitespace. Ever.

### Line lengths

There is no _hard_ line limit. Use as long lines as you want. However, lines that are under 100 characters in length are preferred, simply because I want to be able to reasonably read two files at once in my editor without having to set the font size too low.

There is no need to split long lines to go under 100 characters. We live in 2016 and good wrapping of code is a feature in all editors. However, if you think that readability is improved by wrapping (as with long if statements for example) go ahead and do it.

### Braces

All braces are placed on the their own line. There will be no line of code in the line with the brace. Yes, it uses more space. No, it doesn't really matter. I prefer this because it creates seperation between the code inside the braces and the code outside it, and in my opinion this increases readability. 

This is followed for everything. Functions, loops, conditionals... everything. It doesn't matter if you can skip the braces, you always put them there. 

There is one exception to the "no code on the brace's line" rule, and that is the do-while loop. The `while(...)` statement of the do-while loop can go on the same line as the closing brace.

### Spaces and Stars

There are spaces after basically everything with a bracket next to it other than functions. So, there are spaces after `if` , `switch`,  `case`,  `for`,  `do`,  `while` and `switch`.

The placement of the `*` in code will be next to the type name. This is because it is much more helpful to think of the `*` as part of the type rather than the name of the variable/function. This is a good function:

```c
State* ComputeNextState(State* currentState, double deltaTime);
```

We will place spaces around all binary operators(`+`,  `&`, etc.), and no spaces after any unary operator (`++`, `-` etc). There will however be no spaces next to the `.` and `->` operators for accessing things in structs.

### Naming

All variables get camelCase names `currentState`, `realImportantNumber` etc. All types and functions will get PascalCase names like `ComputeNextState` and `Health`.

Variable names should be nouns, while function names should be verbs.

Use as long names as you want. The number one thing with names is readability. You want to maximize how much information about a variables purpose you can glean from it's name while simultaneously making it easy to read the variable. Use your judgement.

As for structs, `typedef` them. I don't want to type `struct Thing` all the time. Use them `typedefs`.

### Functions

Functions should have a purpose, clearly defined by their name. There shouldn't be functions like `PerformFirstStageOfCalculation`,`PerformSecondStageOfCalculation` and so on.

Functions should be short. Having a 200 line long text dump of a function helps no one. There is very little overhead with calling functions, so spread the logic out in reasonably.

There shouldn't be too many local variables. The television tells me that I can't keep track of more than 7 things at a time, so use that as a ballpark number for the maximum number of local variables. If you need to use more, perhaps it's better to split the function. 

Functions should be seperated from each other by a blank line. They should also be documented very, _very_ well. If I'm using your code, your function documentation is all I'll read, so it should be _good_. Explain what all the arguments are and what the function does. 

In function prototypes, include parameter names. I can't understand what `DoTheThing(int, int, double, double, Whatever);` means.

For error handling in functions, use `goto`s. This is the only place where you should use `goto`s unless you have some complex control flow that you want to exit.

### Commenting

Comment when needed.

Your code should be self documenting. If a quick reading of the code conveys the meaning, then there's no need for commenting. I don't want this:

```c
// Increment things by 1
things++;
```

However, this is very good:

```c
// Compute the next iteration using the current state and cache it
i = ComputeNextIteration(*some, &complex, **function, &call);
```

If it's not obvious already, we're using the new `//` rather than the `/* ... */` for single line comments. For multi line comments, the preferred format is to use multiple `//`s. Basically, you won't really need `/* ... */`.

### Macros and Enums

Macros are capitalized.

```c
#define PI 3.1415
```

You shouldn't use anything with fancy macros, unless you want to mess with anyone who wants to read your code. Macros are for defining global constants. That is it. For local constants use `const`s.

### Miscellaneous

Things that don't fit in the other categories go here.

*   Functions are _never_ passed arrays. Static arrays don't get passed as such in C, they just get converted to bare pointers, and so a `sizeof()` won't work on them to get their size. So, no using things like `void SomeFuncThatAnnoysMe(int a[], int b);`