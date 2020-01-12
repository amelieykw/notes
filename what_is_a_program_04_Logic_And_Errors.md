# Contents

1. [**``What Is A Program?``**](https://medium.com/@rkay301/programming-fundamentals-part-one-what-is-a-program-6e6639aedc58) — A set of instructions to be executed by an Information Processing System
2. [**``The Problem Domain``**](https://medium.com/@rkay301/programming-fundamentals-part-two-the-problem-domain-how-to-design-a-program-application-4faf0a5753f8) — How to design a program/application
3. [**``Storing Information``**](https://medium.com/@rkay301/programming-fundamentals-part-3-storing-information-d450c9cb5fe0) — How to Model Information (data) in an Information Processing System.
4. [**``Logic And Errors``**](https://medium.com/@rkay301/programming-fundamentals-part-4-logic-and-errors-96f818e2e9f6) — The two (primary) types of logic in an Information Processing System; how to handle errors properly
5. [**``Separation Of Concerns``**](https://medium.com/@rkay301/programming-fundamentals-part-5-separation-of-concerns-software-architecture-f04a900a7c50) — The most important Software Architecture principle I have ever come across
6. [**``Proving Programs With Tests``**](https://medium.com/@rkay301/programming-fundamentals-part-6-proving-programs-with-tests-tdd-simple-examples-c501489a4723) — An explanation of the theory, practice, and benefits of testing your software, and applying Test Driven Development.

## Logic And Errors

At least in a general sense, there exists a division of two forms of logic which concern us programmers:

- **``Computation Logic``**: Logic which manipulates real-world information over time (such as performing arithmetic to solve an expression). This is similar to what is called **``“Business Logic”``** in software development jargon.
- **``Control Logic``**: Logic which dictates building, managing, and coordinating the flow of the program itself, such as sending operands and operators to the **``computation logic``**, receiving the result, and passing it to a user interface (or some other output device). This is similar to what is called **``“Application Logic”``** in software development jargon.

I must make one final and rather unfortunate, but very important preamble. It would be really wonderful if we could always keep ``control Logic`` separate from ``computation Logic``, but the truth is that in more complicated programs, it can be tedious and nigh-impossible to do so. In any case, the general prescription to this problem is to rigorously apply **``separation of concerns``**.

 I am secretly telling you to separate out **computational logic functions** to make them what are known as **``“Pure Functions”`` in Functional Programming** jargon.

Example of **computational logic functions**:
```JavaScript
//1 for true, 0 for false
function validateOperator(Character operator) returns: '1' or '0' {
    if (c equals "+") return 1
    if (c equals "-") return 1
    if (c equals "/") return 1
    if (c equals "*") return 1
    return 0
}
function sum(Number operandOne, Number operandTwo) returns: Number 
{
    return operandOne + operandTwo
}
//...and so forth
```

> **computational logic**
>
> “My program will display the result of **``solving``** valid binomial expressions limited to addition, subtraction, multiplication, and division. If an expression is invalid, an error message will be displayed instead of the result of solving the expression.”

Assuming our hypothetical, the above functions could be copy and pasted in to any program ever written (which happens to require solving these same problems), and they would work properly. If they do not work, it is entirely because of a problem with the program or system, which cannot reasonably be accounted in the context of these functions.

> Always do your best to identify and **keep computational logic separate from control logic**!

- In **small scale** programs, this can be achieved by creating a separate Thing (class, struct, object, whatever) and/or Function for each distinct computational operation. It is okay to group conceptually related operations together in Things, but you should still try to break down individual computations into distinct Functions to avoid ugly, hard to read, and hard to test code.
- In **large scale** programs, we can apply these same principles to the different sub-systems (groups of conceptually related Things), giving them specific roles and responsibilities and ensuring that they do not violate them.

> **Control Logic**
>
> “My program will **``display the result``**… If an expression is invalid, **``an error message will be displayed``** instead ”

> Whereas a good indicator that something ought to be considered **``computational logic``**, is that it does **not know or reference anything about the program which uses it, nor the outside world** (such as Input/Output devices like mice and monitors), **``control logic all about that s***``**.

When we say **``“display the result”``**, that implies that there must be some device (or part of the program which does the work of talking to a device) which must ultimately render this result in reality. The simplest form of the output device is a garden variety text-based console or terminal, but if you plan on developing applications for users beyond Linux nerds (which appears to be what I am turning in to gradually), you may need to write different sets of display functions for:

- On-screen widgets, text boxes, labels, graphics, animations, and so forth
- Converting the result into output in an audio format via an enunciator (think text-to-speech)
- All of the above and more, at the same time

There are plenty of other forms which **``control logic``** can take beyond input/output work, but this should be a familiar case to anyone who writes applications which are used by human beings. Other examples include, but are not limited to:

- Sending and receiving information to and from various Things and/or Functions which perform operations like arithmetic and validation
- Deciding what particular user interface screen to show to the user based on the last page they visited, what kind of account they possess, whether their login was successful or not, and so forth.

## Error Handling

Another form of control logic, error handling.

There are only two moving parts here:

- A function is called which may fail to do what you expect it to do, but at least it has the courtesy to tell you of this failure by spitting out an “Exception” or “Error”
- If it does fail, we are given some details about what happened (sometimes exact details, sometimes all we know is that it failed)

The “catch” block is where we tell the program what to do in the event that a failure occurs. Now, it is worth dividing the kinds of failure events into two categories:

- **``Recoverable Errors``**: These kinds of failure events are those which might briefly interrupt the program’s typical/preferred execution, but they do not necessarily require aborting the user’s current progress or killing the program entirely. For example, you are not going to be using a web browser if there is no network connection present, but that does not mean the browser ought to cause a BLUE SCREEN OF DEATH as a result.
- **``Nonrecoverable Errors``**: I think you can guess what this means. Now, sometimes this means to abort the current user session and start anew (which kind of borders on recoverable depending on how patient your user happens to be), but I am mostly thinking of program/device crashing failures. NullPointerException is a classic example, and it is basically is the OS telling you: “Hey dumbass, you told me to look up something in this place in memory, but it did not exist.”

Regarding the errors which you can do something about at runtime, the way in which you determine how to handle the error should be based on asking questions like:

- Does the user need to know something?
- Does the program need to send the user to a specific point in the program’s intended execution (previous page, or previous step in the setup wizard)?
Should the program try the operation a second time, or n number of times until the user closes it?
- Does some transaction of information (such as sending or receiving bank account balances) need to be undone?

It does depend, but the sad truth is that I have seen many developers, and their projects, that could not handle errors properly even if they wanted to. This is because they write programs as giant incoherent blobs which preclude the ability to jump the user back to anything but the very beginning; let alone more complicated situations.

That is of course more of **an architectural concern**, which we discuss in subsequent articles.
