# Lecture Script: Variables and Condition Logic in Colab
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Lecture Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Working in a Colab Notebook — Code Cells and Markdown Cells | 14 min |
| 3 | Variables and Python Naming Conventions | 14 min |
| 4 | Data Types — int, float, str, and bool | 14 min |
| 5 | Converting Between Types | 12 min |
| 6 | Arithmetic Operators, Including Integer Division and Modulo | 14 min |
| 7 | Boolean Expressions — Comparison and Logical Operators | 12 min |
| 8 | Branching with if / elif / else | 17 min |
| 9 | Lecture Summary and Recap | 5 min |

---

## Learning Objectives

By the end of this lecture, learners will be able to:

1. Write and run code in a notebook cell, separating code cells from markdown cells
2. Write a script that stores values in named variables following Python naming conventions
3. Classify a value as int, float, str, or bool
4. Convert a value between string, integer, and float types
5. Compute a result using arithmetic operators, including integer division and modulo
6. Construct a boolean expression using comparison and logical operators
7. Implement branching logic for a stated business rule using if / elif / else

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This is likely a first exposure to Python, and possibly to programming. Assume no prior code experience. The hook should show small, realistic mistakes with real consequences, each one tied to a skill taught today. Open with the question and wait for answers before continuing.

**[Script:]**

"Here is a question to start. A checkout program adds the quantity of one item, 3, to the quantity of another item, 4, and reports a total of 34. What went wrong?

Take a moment. Someone give me a guess.

The answer is that the program was treating 3 and 4 as text, not as numbers. Text gets joined together, so 'three' and 'four' became '34'. Numbers get added, and would have given 7. To a human the two look identical on screen. To Python they are completely different kinds of value.

Here are two more small mistakes with real consequences. A shop has a rule: orders of 100 or more ship free, orders from 50 to 99 pay a small fee, and everything else pays a larger fee. A programmer writes the checks in the wrong order, and every order over 50 pays the small fee, including a 500 order that should ship free. And a beginner runs the cells of a notebook out of order, and the program crashes because a value it needs does not exist yet.

Three mistakes, and each one comes from a basic idea: how a program stores values, what kind of value it is, and how it makes decisions. Those basic ideas are today's lecture. We will work in Google Colab, a free notebook you run in your browser, where you can write Python, run it, and see the result immediately, with nothing to install.

By the end of today you will be able to write and run code in a notebook, store values in well-named variables, recognize and convert between the four basic kinds of value, calculate with arithmetic including two operators you may not have seen before, build true-or-false expressions, and write a program that applies a business rule using if, elif, and else. Everything we do builds toward that last skill."

---

## Block 2 — Working in a Colab Notebook: Code Cells and Markdown Cells

### 2A — Getting Set Up

**[Script:]**

"Google Colab is a notebook environment that runs in your browser. You need a Google account. Go to colab.research.google.com and create a new notebook. You should see a blank notebook with one empty cell."

> 🎯 **Instructor Note:** Treat this as a gate. Do not move on until every learner has a new notebook open with an empty cell visible. Ask: "Raise a hand if you cannot see an empty cell." Resolve sign-in and access problems now; nothing else works without it.

---

### 2B — Two Kinds of Cells

**[Script:]**

"A notebook is a sequence of cells, and a cell is one block you can edit and run on its own. There are two kinds that matter today. A code cell contains Python, and when you run it, Python executes the code and shows the output right underneath. A markdown cell, which Colab calls a text cell, contains formatted text for explanations, headings, and notes, and running it simply displays the formatted text. No Python is executed in a markdown cell."

> 🎯 **Instructor Note:** Write this on the board and keep it visible.

```
Code cell      Python code → run it → output appears below the cell
Markdown cell  Formatted text (headings, notes) → run it → text is displayed
               No Python runs in a markdown cell

Add a cell:    "+ Code" or "+ Text" in the toolbar
Run a cell:    play button on the left of the cell, or Shift + Enter
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I type `print(2 + 3)` into a markdown cell and run it, what do you expect to see?" Answer: the literal text `print(2 + 3)` displayed, not 5, because a markdown cell displays text and never executes it. Let learners commit to an answer before anyone tries it.

**Demo 1 — A first code cell (whiteboard-friendly)**

```python
print("Hello, Colab!")
```

**[Script:]**

"In a code cell, type this and run it with the play button or Shift plus Enter. The output Hello, Colab! appears under the cell. `print()` displays whatever is inside the parentheses. You will use it constantly, because in a notebook only the value of the last line in a cell is displayed automatically. If you want to see something, print it."

**Demo 2 — A markdown cell (whiteboard-friendly)**

```
Add a text cell with "+ Text" and type:

# Order Calculator
This notebook calculates the total for a customer order.

Run the cell (Shift + Enter): it displays a large heading
followed by a plain sentence.
```

**[Script:]**

"In a markdown cell, a pound sign at the start of a line makes a heading. Notice something that trips beginners up: the same pound sign in a code cell means something completely different. In a code cell, `#` starts a comment, a note for humans that Python ignores. The symbol does not have one meaning; its meaning depends on which kind of cell you are in."

> 🎯 **Instructor Note:** Ask: "What is the difference between a heading and a comment, given they both start with a pound sign?" Answer: a heading is a markdown feature for formatting displayed text; a comment is a Python feature that lets a programmer annotate code, and Python ignores it. Which one you get depends on the cell type.

---

### 2C — Notebooks Remember

**[Script:]**

"One more fact about notebooks matters for everything that follows. When you run a code cell, anything it creates stays in memory, and later cells can use it. The notebook remembers across cells. That is powerful, and it is also the source of a classic mistake: if you run a cell that uses a value before you run the cell that creates it, Python reports an error because that value does not exist yet. Run cells in order from top to bottom. If a notebook behaves strangely, restarting the runtime from the Runtime menu clears the memory so you can run everything again cleanly."

**Recap of Block 2 before moving on:**

- A Colab notebook is a sequence of cells that you run individually, with output shown directly beneath each one
- A code cell runs Python; a markdown (text) cell displays formatted text and runs no Python
- `print()` displays a value, and only the last expression in a cell is displayed automatically
- The pound sign is a heading in a markdown cell and a comment in a code cell
- Values created by a cell persist for later cells, so run cells in order from top to bottom

---

## Block 3 — Variables and Python Naming Conventions

### 3A — What a Variable Is

**[Script:]**

"A variable is a name that refers to a stored value. You create one with an equals sign: the name goes on the left, and the value goes on the right. Once it exists, you use the name anywhere you would have used the value, and if the value needs to change, you change it in one place."

> 🎯 **Instructor Note:** Name the first confusion immediately. In Python, a single `=` does not mean 'equals' in the mathematical sense. It means 'store the value on the right under the name on the left.' Say it aloud as 'gets' or 'is assigned': `quantity = 12` reads 'quantity gets 12.'

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the code below with the output line covered. Ask: "What will this print?" Answer: `Notebook 3.5 12 True`. Point out that `3.50` prints as `3.5`; Python does not keep trailing zeros on a number.

**Demo 3 — A script that stores values in named variables (whiteboard-friendly)**

```python
item_name = "Notebook"
unit_price = 3.50
quantity = 12
is_member = True

print(item_name, unit_price, quantity, is_member)
# Notebook 3.5 12 True

quantity = 15
print(quantity)
# 15
```

**[Script:]**

"Four variables, each storing one value. A variable is not permanent: assigning to `quantity` a second time replaced 12 with 15. The old value is gone. Each assignment simply points the name at a new value."

---

### 3B — Naming Conventions

**[Script:]**

"Python has hard rules about names, which cause errors if broken, and conventions, which are agreements among Python programmers that make code readable. Follow both."

> 🎯 **Instructor Note:** Write the rules and conventions on the board.

```
Rules (break these and Python raises an error):
  - Letters, digits, and underscores only; no spaces or hyphens
  - Cannot start with a digit
  - Cannot be a reserved word such as if, else, class, for
  - Case-sensitive: price and Price are different variables

Conventions (Python style guide, PEP 8):
  - Lowercase words joined by underscores: unit_price  (snake_case)
  - Descriptive names: unit_price, not up or x
  - Constants written in capitals: TAX_RATE
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask the room which of these names are valid: `order_total`, `2nd_item`, `customer name`, `Total`, `class`. Answers: `order_total` is valid and follows convention. `2nd_item` is invalid because it starts with a digit. `customer name` is invalid because of the space. `Total` is valid but does not follow the lowercase convention. `class` is invalid because it is a reserved word. Have learners explain each ruling aloud.

**[Script:]**

"A good name tells the reader what the value is. `unit_price` is clear a month from now; `up` is not. Python does not care, but you and everyone who reads your code will."

> 🎯 **Instructor Note:** Mention briefly that names such as `print`, `str`, and `sum` already have meaning in Python; reusing them as variable names can break the original tool later in the notebook. Advise choosing more specific names like `item_name` or `order_sum`.

**Recap of Block 3 before moving on:**

- A variable is a name that refers to a stored value, created with `=` and read as 'gets' rather than 'equals'
- Assigning to an existing variable replaces its value
- Names may use letters, digits, and underscores, cannot start with a digit or contain spaces, cannot be reserved words, and are case-sensitive
- Python convention is lowercase snake_case with descriptive names, and capitals for constants

---

## Block 4 — Data Types: int, float, str, and bool

### 4A — Every Value Has a Type

**[Script:]**

"Remember the opening question about 3 plus 4 giving 34. Every value in Python has a type, and the type decides what you can do with it. There are four basic types for today."

> 🎯 **Instructor Note:** Write this table on the board and keep it visible for the rest of the lecture.

```
Type    Meaning                       Examples
-----   ---------------------------   ---------------------
int     whole numbers                 12, 0, -7
float   numbers with a decimal point  3.5, 0.75, 2.0
str     text, written in quotes       "Notebook", 'A12', "12"
bool    one of two truth values       True, False
```

**[Script:]**

"`int` is for whole numbers. `float` is for numbers with a decimal point, and a number written 2.0 is a float even though its value is a whole number. `str`, short for string, is text, always written inside quotes. `bool`, short for Boolean, has exactly two values, `True` and `False`, and both are written with a capital first letter.

The function `type()` tells you the type of any value."

---

### 4B — Classifying Values

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the four lines below with the comments covered. Then ask: "What type is `"12"`, with the quotation marks? What is `type("3.5")`? What is `type(7.0)`? What is `type(False)`?" Answers: str, str, float, bool. Stress that quotes make a value a string no matter what is inside them.

**Demo 4 — Checking types (whiteboard-friendly)**

```python
print(type(12))        # <class 'int'>
print(type(3.5))       # <class 'float'>
print(type("12"))      # <class 'str'>
print(type(True))      # <class 'bool'>
```

**[Script:]**

"The number 12 is an int, but `"12"` with quotation marks is a str. They may look alike on screen, but Python treats them differently: you can add two ints, but you cannot do arithmetic on two strings in the same way. That is exactly the 34 from the opening: the text values were joined together instead of added."

> 🎯 **Instructor Note:** Run a short classification exercise. Write these on the board and have learners classify each as int, float, str, or bool: `42`, `42.0`, `"42"`, `"True"`, `True`, `-7`, `0.5`, `""`. Answers in order: int, float, str, str, bool, int, float, str. Pay particular attention to `42.0` (a float because of the decimal point), `"True"` (a str because of the quotes, not a bool), and `""` (an empty string is still a str). Common confusions to name: quotes make a str, a decimal point makes a float, and bool values are capitalized `True` and `False` without quotes.

**Recap of Block 4 before moving on:**

- Every value has a type that determines what you can do with it
- The four basic types are int (whole numbers), float (decimal numbers), str (text in quotes), and bool (`True` or `False`)
- Quotes make a value a str regardless of its contents, and a decimal point makes a number a float
- `type()` reports the type of a value; wrap it in `print()` to see the result in a notebook

---

## Block 5 — Converting Between Types

### 5A — Why Conversion Is Needed

**[Script:]**

"Sometimes a value arrives as the wrong type. Data typed into a form, or read from a file, usually arrives as text. If you want to calculate with it, you must convert it. Python has one function for each target type: `int()`, `float()`, and `str()`."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the first three lines and ask: "What does `"5" + "3"` give? What does `5 + 3` give? What does `int("5") + int("3")` give?" Answers: `53`, `8`, `8`. Ask what the plus sign does to two strings versus two ints.

**Demo 5 — Converting types (whiteboard-friendly)**

```python
print("5" + "3")              # 53    (text is joined)
print(5 + 3)                  # 8
print(int("5") + int("3"))    # 8     (text converted to numbers first)
print(float("2.5") * 2)       # 5.0
print(str(25))                # 25    (now a str)
print(int(3.9))               # 3     (drops the decimal part, no rounding)
```

**[Script:]**

"`int("5")` turns the text into the whole number 5. `float("2.5")` turns text into a decimal number. `str(25)` turns a number into text. Notice the last line: `int(3.9)` gives 3, not 4. Converting a float to an int cuts off the decimal part; it does not round."

---

### 5B — When Conversion Fails

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What do you expect to happen when Python tries to convert the text `"3.5"` directly to an int, or the text `"abc"` to an int? And what happens if I try to join text and a number with a plus sign?" Let learners guess that something goes wrong, then reveal the specific errors below.

**Demo 6 — Conversions that raise errors (whiteboard-friendly)**

```python
int("3.5")        # ValueError: invalid literal for int()
int("abc")        # ValueError: invalid literal for int()
"Total: " + 25    # TypeError: can only concatenate str to str

print("Total: " + str(25))    # Total: 25
print(int(float("3.5")))      # 3
```

**[Script:]**

"Two kinds of error appear here. A `ValueError` means the type of the value was acceptable but its content was not: you cannot make a whole number out of the text `"abc"`, or out of `"3.5"` directly. A `TypeError` means the operation does not work for those types: Python will not join text and a number with a plus sign. The fixes follow from what you have learned: convert the number to text with `str()`, or convert in two steps, text to float and then float to int.

Reading error messages is a skill. The last line of the message names the error type and tells you what went wrong, and the lines above it show where. Read it before guessing."

> 🎯 **Instructor Note:** Ask: "Why can't Python convert `"3.5"` straight to an int?" Answer: `int()` on text expects a whole-number string; `"3.5"` has a decimal point, so Python raises an error instead of silently deciding what to do with the decimal part. Converting to a float first makes the intention explicit.

**Recap of Block 5 before moving on:**

- `int()`, `float()`, and `str()` convert a value to that type
- Data from forms and files typically arrives as str and must be converted before arithmetic
- `int()` on a float drops the decimal part and does not round
- A ValueError means the content cannot be converted; a TypeError means the operation is not valid for those types
- Reading the last line of an error message tells you the error type and the cause

---

## Block 6 — Arithmetic Operators, Including Integer Division and Modulo

### 6A — The Operators

**[Script:]**

"Python's arithmetic operators work on ints and floats. Most of them you already know. Two are new to many learners, and they are very useful."

> 🎯 **Instructor Note:** Write this table on the board and keep it visible.

```
Operator   Meaning                          Example    Result
--------   ------------------------------   --------   ------
+          addition                         7 + 2      9
-          subtraction                      7 - 2      5
*          multiplication                   7 * 2      14
/          division (always gives a float)  7 / 2      3.5
**         power                            2 ** 3     8
//         integer (floor) division         7 // 2     3
%          modulo (the remainder)           7 % 2      1
```

**[Script:]**

"Regular division `/` always returns a float: even `6 / 3` is `2.0`. Integer division `//` divides and then drops the fractional part, so `7 // 2` is 3: how many whole times 2 fits into 7. Modulo `%` gives what is left over, so `7 % 2` is 1. The percent sign here has nothing to do with percentages; it means remainder.

Python follows the usual order of operations: parentheses first, then powers, then multiplication, division, integer division, and modulo, then addition and subtraction."

---

### 6B — Using Integer Division and Modulo Together

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show only the first two lines of the demo. Ask: "A video is 130 minutes long. Using `//` and `%` with 60, how many whole hours is that, and how many minutes are left over?" Answer: 2 hours and 10 minutes. Then ask: "What is `17 // 5` and what is `17 % 5`?" Answers: 3 and 2.

**Demo 7 — Integer division and modulo (whiteboard-friendly)**

```python
minutes = 130
hours = minutes // 60
remaining = minutes % 60
print(hours, remaining)          # 2 10

print(7 / 2, 7 // 2, 7 % 2)      # 3.5 3 1
print(17 // 5, 17 % 5)           # 3 2
print(10 % 2, 7 % 2)             # 0 1
```

**[Script:]**

"Integer division and modulo are two halves of the same split. 130 minutes is 2 full hours, with 10 minutes left over. Check it: 2 times 60 plus 10 is 130. For any two whole numbers, the quotient times the divisor, plus the remainder, gives back the original number: 3 times 5 plus 2 is 17.

A practical use of modulo: a number is even when `number % 2` is 0 and odd when it is 1. Another: if you pack 47 items into boxes of 12, `47 // 12` is 3 full boxes and `47 % 12` is 11 items left over."

> 🎯 **Instructor Note:** Name the common confusions: `/` and `//` are different operators, and `%` is a remainder, not a percentage. For negative numbers, `//` rounds down toward negative infinity, so `-7 // 2` is `-4`; mention this only if asked. Also give a short caution about decimals: because computers store floats in binary, `0.1 + 0.2` displays `0.30000000000000004`. For money, this is why results are often rounded for display, for example with `round(value, 2)`. Do not go deeper than this unless a learner asks.

**Recap of Block 6 before moving on:**

- `+`, `-`, `*`, `/`, and `**` are addition, subtraction, multiplication, division, and power; `/` always returns a float
- `//` is integer division: it divides and drops the fractional part
- `%` is modulo: it returns the remainder, and has nothing to do with percentages
- Integer division and modulo together split a quantity into whole units and what is left over
- Parentheses control the order of operations; floats can show tiny rounding differences

---

## Block 7 — Boolean Expressions: Comparison and Logical Operators

### 7A — Comparison Operators

**[Script:]**

"A boolean expression is any expression whose result is `True` or `False`. These are the building blocks of every decision a program makes. The simplest way to create one is to compare two values."

> 🎯 **Instructor Note:** Write this table on the board and keep it visible.

```
Operator   Meaning                    Example      Result
--------   ------------------------   ----------   ------
==         equal to                   5 == 5       True
!=         not equal to               5 != 3       True
>          greater than               5 > 3        True
<          less than                  5 < 3        False
>=         greater than or equal to   5 >= 5       True
<=         less than or equal to      5 <= 3       False
```

**[Script:]**

"Do not confuse `=` and `==`. A single equals sign assigns a value to a variable. A double equals sign asks a question: are these two things equal? Mixing them up is one of the most common beginner errors."

---

### 7B — Logical Operators

**[Script:]**

"Logical operators combine boolean expressions. `and` is True only if both sides are True. `or` is True if at least one side is True. `not` flips a boolean: True becomes False, and False becomes True."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Set `age = 20` and `has_ticket = False` on the board. Ask: "What is `age >= 18`? What is `age >= 18 and has_ticket`? What is `age >= 18 or has_ticket`? What is `not has_ticket`?" Answers: True, False, True, True. Also ask: "What is `"5" == 5`?" Answer: False, because a str and an int are different types and Python does not treat them as equal.

**Demo 8 — Comparison and logical operators (whiteboard-friendly)**

```python
age = 20
has_ticket = False

print(age >= 18)                    # True
print(age >= 18 and has_ticket)     # False
print(age >= 18 or has_ticket)      # True
print(not has_ticket)               # True
print("5" == 5)                     # False  (different types)
```

**[Script:]**

"`age >= 18` is True. With `and`, the whole expression needs both sides True, and `has_ticket` is False, so the result is False. With `or`, one True side is enough, so the result is True. The last line connects back to type conversion: the text `"5"` and the number 5 are not equal, which is exactly why a value that arrives as text must be converted before you compare it."

> 🎯 **Instructor Note:** Ask: "What would `"100" >= 100` do in Python?" Answer: it raises a TypeError, because Python will not compare text with a number using greater-than-or-equal. Converting first with `int("100") >= 100` gives True. Also state the precedence rule: `not` is applied first, then `and`, then `or`. When mixing `and` and `or`, use parentheses so the intent is explicit, and so the next reader does not have to remember the rule.

**Recap of Block 7 before moving on:**

- A boolean expression evaluates to `True` or `False`
- Comparison operators are `==`, `!=`, `>`, `<`, `>=`, and `<=`; `=` assigns and `==` compares
- `and` needs both sides True, `or` needs at least one side True, and `not` flips a boolean
- Values of different types, such as `"5"` and `5`, are not equal, and ordering comparisons across str and int raise an error
- Use parentheses when combining `and` and `or`

---

## Block 8 — Branching with if / elif / else

### 8A — Making a Decision

**[Script:]**

"Everything so far leads here. Branching lets a program take different actions depending on a condition. The condition is a boolean expression, exactly what you just built. Python checks it, and runs the matching block of code.

The structure has three parts. `if` runs its block when its condition is True. `elif`, short for else if, adds further conditions that are checked only if everything before them was False. `else` runs when none of the conditions were True. Python checks from the top down, and the first branch whose condition is True runs. All the rest are skipped."

> 🎯 **Instructor Note:** Write this structure on the board and call out the two syntax details that trip beginners up: each condition line ends with a colon, and the code belonging to a branch is indented, conventionally by four spaces. Python uses indentation to know which lines belong to which branch.

```
if condition_one:
    # runs if condition_one is True
elif condition_two:
    # runs only if condition_one was False and condition_two is True
else:
    # runs if none of the conditions above were True
```

---

### 8B — Implementing a Stated Business Rule

**[Script:]**

"Let us implement a real rule, exactly as a business would state it. Shipping rule: orders of 100 or more ship free. Orders of 50 or more, but under 100, pay 5 for shipping. All other orders pay 10."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "With an order total of 72.50, what shipping fee does the rule give? What about exactly 100? What about 49.99?" Answers: 5, 0, and 10. Have learners trace which condition is the first to be True in each case. Point out why `>=` is used for the 100 boundary: the rule says '100 or more.'

**Demo 9 — The shipping rule (whiteboard-friendly)**

```python
order_total = 72.50

if order_total >= 100:
    shipping = 0
elif order_total >= 50:
    shipping = 5
else:
    shipping = 10

print("Shipping:", shipping)    # Shipping: 5
```

**[Script:]**

"Trace it. Is 72.5 at least 100? No, so we skip to the `elif`. Is 72.5 at least 50? Yes, so shipping becomes 5, and the `else` is skipped. Change `order_total` to 100 and the first condition is True, so shipping is 0. Change it to 49.99 and both conditions are False, so `else` gives 10.

Notice why the elif works without writing 'under 100.' By the time Python reaches it, the first condition has already been False, so the total must already be under 100. The order of the checks carries information."

> 🎯 **Instructor Note:** Ask: "What would go wrong if we wrote the `>= 50` check first and the `>= 100` check second?" Answer: an order total of 500 satisfies `>= 50` first, so it would pay 5 shipping and the free-shipping branch would never run. This is the second mistake from the opening. When ranges overlap, check the most restrictive condition first. Also name two common confusions: `=` instead of `==` inside a condition, and forgetting the colon or the indentation.

---

### 8C — Combining Conditions and Practicing

**[Script:]**

"Now a rule that needs a logical operator and a calculation. Discount rule: members get 10 percent off. Non-members get 5 percent off if the order total is 100 or more. Everyone else gets no discount. Write the code that sets a discount rate, and then computes the final total. Work in pairs for a few minutes."

> 🎯 **Instructor Note:** Run this as a short pair exercise, five minutes. Write the rule and the test cases on the board, then circulate. Ask learners to run all three test cases before comparing answers.

```
Rule:
  Members            → 10% discount
  Non-members, total of 100 or more → 5% discount
  Everyone else      → no discount

Test cases:
  is_member = True,  order_total = 200   → expected final_total 180.0
  is_member = False, order_total = 120   → expected final_total 114.0
  is_member = False, order_total = 80    → expected final_total 80.0
```

> 🎯 **Instructor Note:** A model solution follows. Show it only after pairs have attempted the exercise. Walk through the trace for the second test case: `is_member` is False, so the `if` is skipped; `120 >= 100` is True, so the rate is 0.05, and the final total is 120 times 0.95. Many learners also write a version where the member check and the total check are combined with `and` or `or`; accept any version that passes all three tests, then ask whether the order of the branches mattered.

```python
if is_member:
    discount_rate = 0.10
elif order_total >= 100:
    discount_rate = 0.05
else:
    discount_rate = 0.0

final_total = order_total * (1 - discount_rate)
print(final_total)
```

**[Script:]**

"Check the member case first. A member with a 200 order takes the first branch, so the rate is 0.10, and 200 times 0.9 gives 180. Notice that the member branch does not need to check the total at all: the rule gives members the discount regardless. The `elif` only matters for people who were not members. That is how a stated business rule maps onto if, elif, and else: each sentence of the rule becomes one branch, in an order that makes the later branches correct."

**Recap of Block 8 before moving on:**

- `if`, `elif`, and `else` run different code depending on boolean conditions
- Python checks conditions from the top down, and the first True branch runs; all later branches are skipped
- Each condition line ends with a colon, and the code in a branch is indented
- Because earlier branches have already been False, later conditions need not repeat them
- When ranges overlap, put the most restrictive condition first
- A stated business rule becomes one branch per sentence, tested with cases at the boundaries

---

## Block 9 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as rapid active recall. Ask each question and take answers before confirming. "What is the difference between a code cell and a markdown cell? Which of these names is valid: `total_price` or `2nd_total`? What type is `"12"`, and what type is `12.0`? How do you turn the text `"7"` into a number you can add? What do `//` and `%` each return for `17` and `5`? What is the difference between `=` and `==`? When several `elif` conditions are True, which block runs?" Finish by returning to the three opening mistakes and asking learners to explain how today's ideas prevent each one.

**Working in a Colab Notebook**

- A notebook is a sequence of cells run individually; code cells run Python and markdown cells display formatted text
- `print()` displays values, and cells should be run in order because values persist between cells

**Variables and Naming Conventions**

- A variable is a name that refers to a stored value, created with `=` and replaced by assigning again
- Names use letters, digits, and underscores, cannot start with a digit or be reserved words, and follow lowercase snake_case by convention

**Data Types**

- The four basic types are int, float, str, and bool; quotes make a str and a decimal point makes a float
- `type()` reports a value's type

**Converting Between Types**

- `int()`, `float()`, and `str()` convert values; data from forms and files usually arrives as str
- `int()` on a float drops the decimal part; invalid conversions raise a ValueError or TypeError

**Arithmetic Operators**

- `/` always returns a float; `//` drops the fractional part; `%` returns the remainder
- Integer division and modulo together split a quantity into whole units and a remainder

**Boolean Expressions**

- Comparison operators produce True or False; `=` assigns and `==` compares
- `and`, `or`, and `not` combine booleans; values of different types are not equal

**Branching with if / elif / else**

- The first True branch runs and the rest are skipped, so the order of conditions matters
- A stated business rule translates into one branch per sentence, checked with boundary test cases

**Why All of This Matters Together**

- A program that applies a business rule needs every idea from today working together: values stored in well-named variables, each of the right type or converted to it, arithmetic that produces the numbers, comparisons and logical operators that turn the rule into True-or-False questions, and an if, elif, and else structure that acts on the answers; the three mistakes from the beginning of the lecture, text joined instead of added, conditions checked in the wrong order, and cells run out of order, are each a failure of one of these basics, and being able to recognize them is the foundation for everything more complex you will write in Python

---

*End of script.*
