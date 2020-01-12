# Contents

1. [**``What Is A Program?``**](https://medium.com/@rkay301/programming-fundamentals-part-one-what-is-a-program-6e6639aedc58) — A set of instructions to be executed by an Information Processing System
2. [**``The Problem Domain``**](https://medium.com/@rkay301/programming-fundamentals-part-two-the-problem-domain-how-to-design-a-program-application-4faf0a5753f8) — How to design a program/application
3. [**``Storing Information``**](https://medium.com/@rkay301/programming-fundamentals-part-3-storing-information-d450c9cb5fe0) — How to Model Information (data) in an Information Processing System.
4. [**``Logic And Errors``**](https://medium.com/@rkay301/programming-fundamentals-part-4-logic-and-errors-96f818e2e9f6) — The two (primary) types of logic in an Information Processing System; how to handle errors properly
5. [**``Separation Of Concerns``**](https://medium.com/@rkay301/programming-fundamentals-part-5-separation-of-concerns-software-architecture-f04a900a7c50) — The most important Software Architecture principle I have ever come across
6. [**``Proving Programs With Tests``**](https://medium.com/@rkay301/programming-fundamentals-part-6-proving-programs-with-tests-tdd-simple-examples-c501489a4723) — An explanation of the theory, practice, and benefits of testing your software, and applying Test Driven Development.

> “A program is in essence, a set of instructions to be given to an Information Processing System”

# “Problem Domain” of a program

## What Does The Program Do?

The **first step** in determining the Problem Domain of a program is to ask yourself **``what your application is supposed to do``**.

Since the **goal** of this series is to show you the process of thinking about, designing, and building programs, we will choose a very simple problem for our hypothetical program to solve. For this series, we will build a simple **binomial expression calculator**.

The **first step** in designing any program is to start by **``explaining what it does in simple, written language``**. 

The technical term for this written explanation is a **``“Problem Statement”``**. 

Now, as a complete beginner, I recall being worried about whether or not my problem statements were perfect or not, and the truth is that trying to determine the perfect problem statement (and by extension, problem domain) on your first try, is a fool's errand.

Instead, use this process as a starting point which can be refined as new ideas (or roadblocks) pop up during the design and build process of your program.

### Example 1: **``Rough``** Problem Statement for a Calculator Program

> “My program will solve basic mathematical expressions.”

As you practice this process and build many different programs, you will have a much easier time figuring out good quality problem statements. The above example is rather like what I would have made as a beginner, and we will see shortly that some improvement is required.

In any case, let us discuss ``what we are trying to do by writing problem statements``:

Once you have written down your problem statement, pay very close attention to the verbs and nouns, as these two things will tell you the **information**, and the **functions** (what we do with that information), which your program must have in order to work properly (and therefore solve a problem). In this case, we can start with:

- Mathematical Expression
- Solve Expression

As mentioned before, there is nothing remotely complicated about what we just did; we made a simple description of a program and looked at what things (nouns) and functions (verbs) it will probably require.

However, my experience tells me that we could benefit greatly by being more specific to our problem statement. It is okay to start vague (sometimes you will need to spend a long time researching and refining your understanding of a problem domain), but more questions need to be answered before we are ready to proceed to code:
> - What kinds of mathematical expressions do we want our program to be capable of evaluating?
> - Our job is simple if we just want addition and subtraction, but what about logarithms or cube roots?
> - How many operands (terms) should we allow the user to work with?
> - What about decimals or negative integers?
> - What about invalid expressions? If we try to calculate “1 ÷ 0”, won’t the universe collapse upon itself? How about if the user types 0.0.0.0 + 1?

The point I am trying to make here is that our problem statement, upon closer analysis, really is quite vague.

Now, I must mention that **``our goal at this stage is not to figure out every single detail and include it in the problem domain before we write any code (paralysis by analysis)``**, but we can save ourselves a great deal of a headache just by spending a few minutes asking important questions like the ones above.

### Example 2: Refined Problem Statement for a Calculator Program

> “My program will display the result of solving valid binomial expressions limited to addition, subtraction, multiplication, and division. If an expression is invalid, an error message will be displayed instead of the result of solving the expression.”

That sounds much better to me. As you can see, we have essentially just tried to be more specific about what our program will and will not do.

Let us take a second look at ``what information our program will need to hold as values (not changeable) and variables (changeable)``…:

- Two operands (terms) which will presumably be entered by the user
- An operator to specify one of addition, subtraction, multiplication, or division
- An error message to tell the user if the expression turns out to be invalid
- The result of solving the expression, assuming it is valid

…And what our program will need to do with that information:

- Ensure that the expression is valid
- Solve a valid expression
- Display the result of solving a valid expression
- Display an error message if an expression turns out to be invalid

As we will see in the next article, **the above information (which is the result of analyzing our program’s problem domain)**, will give us what we need to start building our program with a solid foundation.

> an easy way to balance these concerns (not to mention justify subscription based payments from your users), is :
> 
> - **``to start with basic features, and gradually release your program with new features``** as you build them.
> - **``to start with only the most fundamentally important features``** for your program, and be prepared to change things as you build them out!

The second bit of good news is that in part 5, I will show you how to build programs that are easy to change and improve over time. Do not listen to people who say that software architecture does not matter; that statement is as silly as claiming that building architecture does not matter.

## Summary

The first step in writing a program is to determine what kind of information our program will need to represent digitally (such as mathematical operands and operators), and to know what functions we will need to apply to that data (such as addition, or printing an error message to the screen). I refer to this collectively as the problem domain, and a great way to figure out what it looks like is to start with a problem statement!

Now, once you begin to write more complicated applications that solve more than one problem, you will want to look into things like **``Use Cases``** and **``User Stories``**, which allow you to be more practical and systematic in the way you design complex applications. In any case, it always comes back to problem domain analysis of one form or another, and problem statements are a great start.
