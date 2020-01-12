# Contents

1. [**``What Is A Program?``**](https://medium.com/@rkay301/programming-fundamentals-part-one-what-is-a-program-6e6639aedc58) — A set of instructions to be executed by an Information Processing System
2. [**``The Problem Domain``**](https://medium.com/@rkay301/programming-fundamentals-part-two-the-problem-domain-how-to-design-a-program-application-4faf0a5753f8) — How to design a program/application
3. [**``Storing Information``**](https://medium.com/@rkay301/programming-fundamentals-part-3-storing-information-d450c9cb5fe0) — How to Model Information (data) in an Information Processing System.
4. [**``Logic And Errors``**](https://medium.com/@rkay301/programming-fundamentals-part-4-logic-and-errors-96f818e2e9f6) — The two (primary) types of logic in an Information Processing System; how to handle errors properly
5. [**``Separation Of Concerns``**](https://medium.com/@rkay301/programming-fundamentals-part-5-separation-of-concerns-software-architecture-f04a900a7c50) — The most important Software Architecture principle I have ever come across
6. [**``Proving Programs With Tests``**](https://medium.com/@rkay301/programming-fundamentals-part-6-proving-programs-with-tests-tdd-simple-examples-c501489a4723) — An explanation of the theory, practice, and benefits of testing your software, and applying Test Driven Development.

# What Is A Program (Literally)?

A program is a set of instructions to be given to an Information Processing System (or IPS for short). 

We will begin by looking at each term in the name “**``Information Processing System``**” in isolation, as they point to **descriptions, requirements, and limitations** of modern programs as well as the machines which interpret them.

## Information

Take the following pieces of information, which are conveyed to you, via the English language, Arabic numerals, and the mathematical “+” operator:

> (x) — “Assuming Base 10 Arithmetic, 1 + 1 is equal to 2”
>
> (y) — “The Earth Orbits the Sun”

From the above examples, it is my assessment that information typically possesses the following qualities:

Firstly, information must have :

- some relationships to observations of physical reality, such as y
- some relationships to abstractions which are derived from physical reality such as x.

**``Abstractions``** are used by animals and machines alike, to represent information (facts, details, circumstances) and patterns which are present in reality (admitting that our abstractions may be false, inaccurate, or useless). They do not need to look the same aesthetically, but they can be verified to be true or false based on whether or not they appear consistent with a detailed and objective analysis of reality.

For example, I look down at my hands and notice that I have two of them (at least at this moment in time). By convention, I would represent the number of hands that I possess, as the Arabic numeral “2”. However, if an ancient Roman were to look down at his or her hands, they might by convention arrive at “I I ” as the appropriate abstraction to represent what is the same quantity. I must point out that my convention of using “2”, versus an ancient Roman using “I I ”, versus a computer using “10” (which is equivalent to 2 in binary; being a base 2 counting system), does not imply that my convention is superior, or more correct.

At this present moment in history (although I suspect this may change in under 50 years), humans are capable of representing information not just in letters and numerical symbols, but they can do so through two dimensional, three dimensional, auditory, kinesthetic, and other forms of abstractions.

> Computers, by contrast, is currently limited to representing information as collections of ``ON`` and ``OFF`` values (or ``0`` and ``1``), do not possess the same capacity.

The challenge for us programmers then is ***to find ways to store, create, examine, and compute complex information in a way***.

## Processing

> Processing === that information stored in memory can be manipulated (changed, combined, inputted, outputted, and so forth) according to specific rules which can also be stored in the system.

## System

Be it an animal or machine, an Information Processing System appears to have a set of requirements necessary in order to function:

- **``Memory``**, in order to store information either temporarily, during run time (discussed later), or for the entire duration of the IPS’ existence
Decision-making capacity, in order to solve problems based on ***what information and or events are currently relevant to solving the current problem***
- **``Input and Output devices``**, to supply new information to the IPS, and to be able to see the results of processing this information

These fundamental parts of an IPS were best described in what is known as the **``Von Neumann Architecture``** (the above list of requirements I gave is a slight generalization of it), which is still used by most modern computers:
![Von Neumann Architecture](https://miro.medium.com/max/1920/1*SFOGAx4E6yQqSITTTXAo0A.png)
