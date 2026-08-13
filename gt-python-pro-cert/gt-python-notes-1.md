# Computing in Python I: Fundamentals and Procedural Programming

### **NOTE**: numbering is unit, chapter, lesson, video segment (e.g., 1.1.7.0 => Unit 1, Chapter 1, Lesson 7: Python, 0th video) 

## Chapter 1.1.: Computing

- Mostly review of basic programming and computing concepts
- Helpful reminder of compiled (static) vs interpreted (dynamic) languages
    - analogy --- checking if directions make sense vs. following them

- Course Outline:
    - Unit 1: Basics
    - Unit 2: Procedural Programming (variables, logical and mathematical operators)
    - Unit 3: Control Structures (conditionals, loops, functions, exception handling)
    - Unit 4: Data Structures (strings, lists (arrays), file input/output, dictionaries)
    - Unit 5: Objects & Algorithms (introduction to OOP, searching sorting algos, etc.)

- Python (1.1.7)
    - dynamic, high-level (i.e., more portable, less worry about managing memory or having to be tied to certain operating systems like low-level languages )
    - using Python 3 (not compatible w/ Python 2)
    - dynamic + **interpreted** --- runs line by line, without compiling it first
    - compiled languages check for errors before running code; w/ interpreted languages (Python, JS), you can add tools/functionality to do that (e.g., TypeScript for JS)


## Chapter 1.2: Programming

- Writing code -> executing/running it -> evaluating on results
- Review of basic coding principles, print() function, primitive data types (strings, numbers, floats, Boolean) etc.

- Running Code (1.2.4)
    - **Compiling** code vs **executing** code
        - Complitation:
            - Static  (Compiled) --- Java or C
                - Entire program is compiled before it is run
                - Prechecking for errors, in a practical sense
            - Dynamic (Interpreted) --- no compilation required, run lines one by one; e.g., Python, JS
                - Lines are compiled and run one at a time
        - Execution:
            - Building a table analogy --- compilation would be checking you have all the parts beforehand, but misassembled due to bad instructions (e.g., bug or error)
                - There can be run-time errors, there can be correct outcomes, there can also be incorrect outcomes (despite the code executing successfully --- i.e., no errors but it doesn't what you didn't expect)
    ...
- Executing Code in Python (1.2.5)
    ...
- "Compiling" Python (1.2.5.2)
    - **PyCharm** provides some error detection, spelling mistakes, etc. prior to running code --- mimics compilation in the dev environment
    - simluates compilation in a sense

- The Python Interactive Mode (1.2.5.3)
    - Similar to the console in the browser for running quick JS snippets
    - Scripting mode is your usual move


## Chapter 1.3: Debugging

- Good review
- Compilation errors --- syntax, name, type errors
- Runtime errors --- divide by zero, null errors (cod w/ variables that has no value), memory (code exceeds your PC's memory)

- Types of Errors in Python (1.3.3)
    - SyntaxError
    - NameError
    - TypeError
    - AttributeError (i.e., when code asks for info about a variable that doesn't make sense)
    - "Traceback" starts any error message in the terminal or console in Python

- Basic Debugging (1.3.4)
    - Print debugging --- use `print` statements to show you parts of the code's flow
    - Scope debugging --- add `print` statements to check status of variables in the program at diff stages to see how they're changing
    - Rubber ducking --- explaining the problem from scratch often reveals the solution

- Basic Debugging in Python (1.3.5)
    - Scope debugging example --- average grades in class. (His term --- essentially, chunking)

- Advanced Debugging (1.3.6)
    - "Good to know about" --- step-by-step, variable visualization, in-line debugging

- Fun resource: The CS1301.com Debugging guide (http://cs1301.com/debugging/)


## Chapter 2.1: Procedural Programming

- Procedural --- giving instructions to the computer to carry out an action(s)

- Functional Programming (2.1.1)
    - Function: A segment of code that performs a specific task, sometimes taking some input and sometimes returning some output
    - Method: A function that is part of a class in object-oriented programming (but colloquially, often used interchangeably with function)

- Object-Oriented Programming (2.1.1.2)
    - A programming paradigm where programmers define custom data types that have custom methods embedded within them

- Event-Driven Programming (2.1.1.3)
    - A type of programming where the program generally awaits and reacts to events rather than running code linearly

- Data Types, Variables, Logical (`==`, `>`, `<`, `>=`, `<=`, `and`, `or`, `not`) & Mathematical Operators overviews (2.1.2)

- Comments and Documentation (2.1.3), in Python (2.1.4)
    - Use `#`
    - In-line
    - Code block style (no special syntax for this like in JS with `/*... */`)
    - Use intuitive, informative variable naming

- Neat supplemental for writing scripts - https://shrew.app/lessons/introduction/ 


## Chapter 2.2.: Variables

- Review
- Unlike JS, no keyword needed for variable assignment
- Underscore ( _ ) is convention for Python
- With **Booleans**, remember to **capitalize** `True` and `False`
- Re-assignment is easy (2.2.2.2)
- "Self-documenting code" --- i.e., name your variables intelligently (indicate data it holds, type perhaps)
- Variable names --- no spaces, start with a letter, avoid keywords, use underscores
- RE: null pointer exception --- sort of like error; e.g., when a value hasn't been assigned to a variable

- Basic Data Types (2.2.5)
     - Integers (whole numbers)
     - Real numbers
     - Characters
     - Strings
     - Booleans

- Data Types in Python (2.2.6)
    - Recall that it's a **weakly typed** language (like JS)
    - Contrast w/ strongly typed (e.g. Java, C++ or even TS) where assigning a type is a separate and mandatory step

    - Common Types (2.2.6.1 - 2)
        - int, float, str, bool --- `class` is in output when you use the built-in `type()` function
    - Mixing Types (2.2.6.3)
        - Interesting behavior when multiplying different primitives (e.g., string * Boolean, float or int * string, etc.; only float * string throws an error)

- Interlude: `None` and `None` Type
    - a.k.a, **`null`** in Python
    - Variable exists, but has no value
    - Another example of `None` at work:

    ```py
    a = "Hello"
    b = "world"
    c = a + b
    d = print(c)
    print(d)
    ```

    - Console/output: `Helloworld` and `None` (the `print(c)` statement actually logs the string, but `d`'s value isn't assigned to anything, so it's given `None`)

- Type Conversions (2.2.7)

    - Converting a number to string type with `str()`

        ```py
        my_number = 5
        print(type(my_number))
        my_number_as_string = str(my_number)
        print(type(my_number_as_string))

        ## output -> <class 'int'> <class 'str'>
        ```

        - Python will implicitly infer/coerce type to string in typical/basic cases:
        ```py
        from datetime import date
        my_date = date.today()
        print("Today's date: ", my_date)
        ```

        ```py
        ## example where it has to be done manually: string concatenation (see the print statement)
        ## without the str() conversion of the date, Python would throw a TypeError -> can't infer date as string type implicitly
        from datetime import date
        my_date = date.today()
        my_date_as_string = str(my_date)
        print("Today's date: " + my_date)

    - Functions can be used inline (generally, Python runs/evaluates expressions from innermost to outermost)
    
    - Converting from Strings
         - If strings are made up of only digits, then `int()` and `float()` will convert them into integers and floats (decimals)
         - If given a float, `int()` will throw a `ValueError` since it can't convert a decimal into a whole number (integer)
         - With `bool()`, generally any string will be `True`, but 0 or an empty string will result in `False`

    - Reserved Keywords in Python (2.2.8)
        - .