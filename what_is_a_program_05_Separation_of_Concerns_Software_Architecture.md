# Contents

1. [**``What Is A Program?``**](https://medium.com/@rkay301/programming-fundamentals-part-one-what-is-a-program-6e6639aedc58) — A set of instructions to be executed by an Information Processing System
2. [**``The Problem Domain``**](https://medium.com/@rkay301/programming-fundamentals-part-two-the-problem-domain-how-to-design-a-program-application-4faf0a5753f8) — How to design a program/application
3. [**``Storing Information``**](https://medium.com/@rkay301/programming-fundamentals-part-3-storing-information-d450c9cb5fe0) — How to Model Information (data) in an Information Processing System.
4. [**``Logic And Errors``**](https://medium.com/@rkay301/programming-fundamentals-part-4-logic-and-errors-96f818e2e9f6) — The two (primary) types of logic in an Information Processing System; how to handle errors properly
5. [**``Separation Of Concerns``**](https://medium.com/@rkay301/programming-fundamentals-part-5-separation-of-concerns-software-architecture-f04a900a7c50) — The most important Software Architecture principle I have ever come across
6. [**``Proving Programs With Tests``**](https://medium.com/@rkay301/programming-fundamentals-part-6-proving-programs-with-tests-tdd-simple-examples-c501489a4723) — An explanation of the theory, practice, and benefits of testing your software, and applying Test Driven Development.

# Separation Of Concerns

previous sidewalk:
![previous sidewalk](https://miro.medium.com/max/1697/1*gH4P7RghPdlh3YOESDWINQ.jpeg)
current sidewalk:
![current sidewalk](https://miro.medium.com/max/1200/1*Rek7rfwNLrMyXCEzKVx-zg.jpeg)

> "what the benefit of separating a continuous sidewalk into sections"

might be:

- Supposing that a section was to become cracked or destroyed, one would only need to replace a single section to restore the sidewalk to its former shape and function
- One could build each segment of the sidewalk in isolation of the others

I will not carry this analogy further, but I would like you to keep it in mind as we now move to a more technical exposition of **``separation of concerns``**.

## Separations In ``Graphical User Interface`` Applications

 [“The Perfect Model-View-Whatever Architecture.” (MVP/MVC/MVVM)](https://medium.com/@rkay301/the-perfect-model-view-whatever-architecture-by-ryan-kay-mvp-mvc-mvvm-255cf5fe4421)

 The common thread in most of these architectures, was a separation of three kinds of code:

- **``User Interface/View``**: Code which interacts with what the user sees, and can interact with
- **``Logic``**: Code which ``handles events`` and coordinates the ``flow of data`` between various parts of the program
- **``Data/Model``**: Code which is primarily concerned with ``storage`` and ``retrieval`` of real-world information (which I will refer to as “data” from here on)

> In any case, I will now discuss the separation of concerns as applied to three different perspectives of software architecture:
>
> - **Functions/Methods/Algorithms**
> - **Things/Classes/Objects**
> - **Modules/Components/Sub-systems/Packages**

## Separation Of ``Functions``

> Try not to write giant functions.

The following function accepts three String arguments (Strings are collections of characters, numbers, and symbols such as “Hello World” or “14.500001”), checks whether they are valid to be used as a mathematical expression and if so, returns a String result of evaluating this expression. If any of the arguments are not valid, it returns a String error message:

```Kotlin
/* To represent a mathematical binomial expression, we will use this class (a "class" is Kotlin's version of a "Thing" as discussed in Part 3*/
class Expression(
    val operandOne: Double,
    val operandTwo: Double,
    val operatorSymbol: String
)
/**
 * Returns the result of a valid binomial expression, or an error     message if the expression is invalid
 */
fun solveBinomialExpression(operandOne: String, 
                            operandTwo: String, 
                            operatorSymbol: String): String {
    //1 Validate arguments (toDoubleOrNull is a function within the Kotlin Standard Library)
    val ERROR_MESSAGE = "An error has occured."

    if (operandOne.toDoubleOrNull() == null) return ERROR_MESSAGE
    if (operandTwo.toDoubleOrNull() == null) return ERROR_MESSAGE

    val expression: Expression

    when (operatorSymbol) {
        "+" -> expression = Expression(operandOne.toDouble(), operandTwo.toDouble(), operatorSymbol)
        "-" -> expression = Expression(operandOne.toDouble(), operandTwo.toDouble(), operatorSymbol)
        "*" -> expression = Expression(operandOne.toDouble(), operandTwo.toDouble(), operatorSymbol)
        "/" -> expression = Expression(operandOne.toDouble(), operandTwo.toDouble(), operatorSymbol)
        else -> return ERROR_MESSAGE
    }

    //2 Calculate Result

    when (expression.operatorSymbol) {
        "+" -> return (expression.operandOne + expression.operandTwo).toString()
        "-" -> return (expression.operandOne - expression.operandTwo).toString()
        "*" -> return (expression.operandOne * expression.operandTwo).toString()
        "/" -> return (expression.operandOne / expression.operandTwo).toString()
        else -> return ERROR_MESSAGE
    }
}
```

In the next example, observe that we have **divided this single function**, into a primary function, with **a series of ``“helper functions”``** :

```Kotlin
class Expression(
        val operandOne: Double,
        val operandTwo: Double,
        val operatorSymbol: String
)
//Error message has been defined outside of our primary function
const val ERROR = "An error has occured."
/**
 * Returns the result of a valid binomial expression, or an error message if the expression is
 * invalid */
fun solveBinomialExpression(args: Array<String>): String {
//Looks cleaner and more legible in my estimation
    val expressionResult = validateArguments(args[0], args[1], args[2])

    if (expressionResult == null) return (ERROR)
    else return evaluateExpression(expressionResult)

}
//this function, might return an expression, or it might return "null"
//if the arguments are valid, we return expression; else return null
fun validateArguments(operandOne: String, operandTwo: String, operatorSymbol: String): Expression? {
    if (operandOne.toDoubleOrNull() == null) return null
    if (operandTwo.toDoubleOrNull() == null) return null

    return when (operatorSymbol) {
        "+" -> Expression(operandOne.toDouble(), operandTwo.toDouble(), operatorSymbol)
        "-" -> Expression(operandOne.toDouble(), operandTwo.toDouble(), operatorSymbol)
        "*" -> Expression(operandOne.toDouble(), operandTwo.toDouble(), operatorSymbol)
        "/" -> Expression(operandOne.toDouble(), operandTwo.toDouble(), operatorSymbol)
        else -> null
    }
}
fun evaluateExpression(expression: Expression): String {
    return when (expression.operatorSymbol) {
        "+" -> (expression.operandOne + expression.operandTwo).toString()
        "-" -> (expression.operandOne - expression.operandTwo).toString()
        "*" -> (expression.operandOne * expression.operandTwo).toString()
        "/" -> (expression.operandOne / expression.operandTwo).toString()
        else -> ERROR_MESSAGE
    }
}
```

We have hidden the ugly stuff behind helper functions based on what that logic is concerned with. This allows ``solveBinomialExpression(…)`` to act as a ``control logic based function`` which ``delegates computational logic to helper functions``.



## Separation Of ``Things``



## Separation Of ``Modules``



## Why Bother Doing This?