# Contents

1. [**``What Is A Program?``**](https://medium.com/@rkay301/programming-fundamentals-part-one-what-is-a-program-6e6639aedc58) — A set of instructions to be executed by an Information Processing System
2. [**``The Problem Domain``**](https://medium.com/@rkay301/programming-fundamentals-part-two-the-problem-domain-how-to-design-a-program-application-4faf0a5753f8) — How to design a program/application
3. [**``Storing Information``**](https://medium.com/@rkay301/programming-fundamentals-part-3-storing-information-d450c9cb5fe0) — How to Model Information (data) in an Information Processing System.
4. [**``Logic And Errors``**](https://medium.com/@rkay301/programming-fundamentals-part-4-logic-and-errors-96f818e2e9f6) — The two (primary) types of logic in an Information Processing System; how to handle errors properly
5. [**``Separation Of Concerns``**](https://medium.com/@rkay301/programming-fundamentals-part-5-separation-of-concerns-software-architecture-f04a900a7c50) — The most important Software Architecture principle I have ever come across
6. [**``Proving Programs With Tests``**](https://medium.com/@rkay301/programming-fundamentals-part-6-proving-programs-with-tests-tdd-simple-examples-c501489a4723) — An explanation of the theory, practice, and benefits of testing your software, and applying Test Driven Development.

## Storing Information

The problem with explaining things is that it can be very difficult to discuss or define one thing, without using the definitions of other things which the student may not be aware of. 

1. One solution to this problem, is to attempt to define new things only with reference to things which the average person (who does not necessarily have any experience with the thing) is likely to be familiar with.
2. Another way of explaining something in a way which does not necessitate as much assumed knowledge and technical definitions, is to show an example of the thing itself, and make observations about it.

It is very important to also tell the computer if the information is a:

- ``Value``: Something which does not change during the course of the program’s execution, the constant π (think immutable, final, constant, static)
- ``Variable``: Something is likely to change during the course of the program’s execution, such as the current system time in milliseconds.

If us programmers are smart enough to tell the computer to do so, the computer typically has ``special places in memory ``space to store ``Values``, which results in more efficient programs, and by extension, a better user experience!

If we pretend that this store is actually the memory space of a program, then a well-ordered store would group conceptually related items in to “sections” and “departments”.

> Grouping conceptually related information in “Things”, actually helps the computer build a more efficient memory space in the same sense of my analogy.

## Summary

As a programmer, you will almost invariably find yourself instructing the computer to create “Things” which **``encapsulate (separate) information``**, as well as ``functions`` (which we will discuss in **Part 4: Logic And Errors**).
