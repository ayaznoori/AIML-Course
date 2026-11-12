# Lecture Script: Functions for Reusable Code and Reading Classes
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Lecture Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Writing Functions — Parameters, Arguments, and return | 22 min |
| 3 | Calling Functions — Positional and Keyword Arguments | 15 min |
| 4 | Default Parameter Values | 13 min |
| 5 | Scope — Why a Variable Inside a Function Is Not Visible Outside | 17 min |
| 6 | Reading Classes — Definitions and Instances | 27 min |
| 7 | Lecture Summary and Recap | 8 min |

---

## Learning Objectives

By the end of this lecture, learners will be able to:

1. Write a function that takes arguments and returns a computed value
2. Call a function using positional and keyword arguments
3. Write a function with default parameter values
4. Explain why a variable defined inside a function is not visible outside it
5. Explain the difference between a class definition and an instance created from it

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience already knows variables, the basic types, arithmetic, boolean expressions, if / elif / else, for and while loops, and tracing, and works in Google Colab notebooks. Functions and classes are the first tools for organizing code instead of just writing it. The hook should show a realistic cost of not having them. Note the emphasis in the title: for classes, the goal is reading and understanding, not building elaborate designs. Open with the question and wait for answers before continuing.

**[Script:]**

"Here is a situation. A shop's notebook calculates the shipping fee for an order in five different places: the checkout summary, the daily report, the refund tool, the weekly export, and a customer email. Each one has the same block of if and elif statements, pasted in. Then the business changes the rule: free shipping now starts at 150 instead of 100.

What can go wrong? Take a moment and think about it.

A programmer updates four of the five copies. The fifth still uses 100. Now the daily report and the customer email disagree about the same order, and nobody notices until a customer complains. Nothing crashed. The program simply contains the same rule in five places that no longer agree.

A function fixes this. You write the rule once, give it a name, and every place that needs it calls that name. Change the rule in one place, and every caller is updated.

Today covers how to write functions, how to hand them values and get results back, how to call them in different ways, how to give them default values, and one rule that surprises almost every beginner: a variable created inside a function cannot be seen from outside it. We will see exactly why, and why that is a feature, not a flaw.

Then we turn to classes. Real code, in libraries and in your teammates' notebooks, is built from functions and classes, and reading them is as important as writing them. You have already met classes without knowing it: when you ran `type("cat")` and Python reported `str`, that was a class. By the end of today you will be able to read a simple class, say what it describes, and explain the difference between the class itself and the objects created from it."

---

## Block 2 — Writing Functions: Parameters, Arguments, and return

### 2A — The Shape of a Function

**[Script:]**

"A function is a named, reusable block of code. You define it once, using the keyword `def`, and you use it as often as you like. It can take values in, called parameters, and it can hand a value back, using `return`."

> 🎯 **Instructor Note:** Write this on the board and keep it visible for the whole lecture. Say aloud the two verbs: you define a function, and you call a function. Defining does not run the body.

```
def function_name(parameter_one, parameter_two):
    # the body: indented code that uses the parameters
    return result

- def starts the definition; the line ends with a colon
- parameters are names for the values the function will receive
- return hands a value back to whoever called the function
```

**[Script:]**

"Two words are often confused. A parameter is the name in the definition: a placeholder. An argument is the actual value you pass in when you call the function. In `total_price(3.50, 12)`, the values 3.50 and 12 are arguments; the names `unit_price` and `quantity` in the definition are parameters."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the definition and the call separately. First ask: "If I run only the cell with the `def` and nothing else, what do I see?" Answer: nothing. Defining stores the function but does not run it. Then ask what the call prints. Answer: 42.0. Remind learners that in a notebook a cell containing `def` must be run before any later cell can call the function.

**Demo 1 — Defining and calling a function (whiteboard-friendly)**

```python
def total_price(unit_price, quantity):
    return unit_price * quantity

result = total_price(3.50, 12)
print(result)        # 42.0
print(total_price(0.75, 40))    # 30.0
```

**[Script:]**

"The function receives 3.5 and 12, computes the product, and returns 42.0, which we store in `result`. The same function works for 0.75 and 40 with no new code. That reuse is the point."

---

### 2B — return Versus print

**[Script:]**

"A very common confusion is the difference between `return` and `print`. They look similar when you run them, but they do different things. `print` displays a value on the screen and then the value is gone. `return` hands the value back to the code that called the function, where it can be stored in a variable, used in a calculation, or passed to another function."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the demo with the comments covered. Ask: "What do `a` and `b` contain at the end, and what does the last line print?" Answer: `a` is `None`, `b` is 42.0, and the last line prints `None 42.0`. The first function displays 42.0 but returns nothing, so Python hands back the special value `None`.

**Demo 2 — print compared with return (whiteboard-friendly)**

```python
def show_total(unit_price, quantity):
    print(unit_price * quantity)

def get_total(unit_price, quantity):
    return unit_price * quantity

a = show_total(3.50, 12)    # displays 42.0
b = get_total(3.50, 12)     # displays nothing
print(a, b)                 # None 42.0
```

**[Script:]**

"`show_total` printed 42.0 to the screen, but it returned nothing, so `a` holds `None`. `get_total` printed nothing, but it gave the value back, so `b` holds 42.0 and can be used again. A function that produces a result for other code to use should `return` it. A function without a `return` statement returns `None`."

---

### 2C — return Inside Branches and Loops

**[Script:]**

"Everything you already know works inside a function. Here is the shipping rule from the beginning of the lecture, written once as a function. A `return` ends the function immediately, so each branch can return its own answer."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What are the fees for 72.50, for 100, and for 49.99?" Answer: 5, 0, and 10. Ask learners to name the branch that runs for each, using the method of testing conditions from the top down. Point out that there is no `return` after the if-chain: the function ends the moment any `return` runs.

**Demo 3 — A business rule as a function (whiteboard-friendly)**

```python
def shipping_fee(order_total):
    if order_total >= 100:
        return 0
    elif order_total >= 50:
        return 5
    else:
        return 10

print(shipping_fee(72.50), shipping_fee(100), shipping_fee(49.99))   # 5 0 10
```

**[Script:]**

"Now the rule lives in one place. If the business changes the threshold, you edit one line, and every caller gets the new behavior. That is the answer to the question we opened with.

Loops work inside functions too. Here is the accumulator pattern from earlier, packaged for reuse."

**Demo 4 — A function that contains a loop (whiteboard-friendly)**

```python
def total_of(prices):
    total = 0
    for price in prices:
        total += price
    return total

print(total_of([3.50, 0.75, 24.00]))    # 28.25
```

> 🎯 **Instructor Note:** Ask: "Why is the `return` after the loop and not inside it, at the same indentation as `total += price`?" Answer: inside the loop, the function would return after the first price and ignore the rest. Placed after the loop, it returns once, when the loop is finished.

**Recap of Block 2 before moving on:**

- A function is defined once with `def`, a name, parameters, and an indented body, and then called by name
- Defining a function does not run it; in a notebook the defining cell must be run before later cells can call it
- A parameter is the name in the definition, and an argument is the value passed in a call
- `return` hands a value back to the caller; `print` only displays it, and a function with no `return` gives back `None`
- A `return` ends the function immediately, and conditionals and loops work inside functions as usual

---

## Block 3 — Calling Functions: Positional and Keyword Arguments

### 3A — Two Ways to Pass Arguments

**[Script:]**

"When you call a function, there are two ways to match arguments to parameters. Positional arguments are matched by position: the first argument goes to the first parameter, the second to the second, and so on. Keyword arguments are matched by name: you write the parameter name, an equals sign, and the value."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the function and the first two calls. Ask: "These two calls use the same two numbers, only in a different order. Do they give the same result?" Answer: no. 27.5 and -27.5. Then ask what the keyword versions do. Let learners commit to answers before you reveal each line.

**Demo 5 — Positional and keyword calls (whiteboard-friendly)**

```python
def change_due(paid, total):
    return paid - total

print(change_due(100, 72.50))               # 27.5
print(change_due(72.50, 100))               # -27.5   (order matters)
print(change_due(total=72.50, paid=100))    # 27.5    (names, in any order)
print(change_due(100, total=72.50))         # 27.5    (positional first, then keyword)
```

**[Script:]**

"With positional arguments, order is everything: swapping the two values swaps their meaning, and the result goes negative without any error to warn you. With keyword arguments, the name decides, so the order does not matter, and the call explains itself to anyone reading it. You can mix the two, but positional arguments must come first.

Keyword arguments are especially helpful when a function has several parameters of the same type, like two prices: `change_due(paid=100, total=72.50)` is unambiguous."

---

### 3B — When a Call Goes Wrong

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the three calls and ask: "Which kinds of mistake do these represent, and what do you think Python will say?" Learners should expect an error each time. Then reveal. Remind them to read the last line of the error message, which names the problem and the function.

**Demo 6 — Common calling errors (whiteboard-friendly)**

```python
change_due(100)
# TypeError: missing 1 required positional argument: 'total'

change_due(100, 72.50, 5)
# TypeError: takes 2 positional arguments but 3 were given

change_due(paid=100, totl=72.50)
# TypeError: got an unexpected keyword argument 'totl'
```

**[Script:]**

"Too few arguments, too many arguments, and a misspelled parameter name each produce a clear message. Read it: it names the parameter that is missing, the count that was wrong, or the keyword Python did not recognize. Because the function's parameters are fixed by its definition, every call must supply what the definition asks for."

**Recap of Block 3 before moving on:**

- Positional arguments are matched to parameters by position, so their order matters
- Keyword arguments are matched by name, so their order does not matter and the call is easier to read
- Positional arguments must come before keyword arguments in a call
- Missing, extra, or misspelled arguments raise a TypeError whose last line names the problem

---

## Block 4 — Default Parameter Values

### 4A — Giving a Parameter a Standard Value

**[Script:]**

"Sometimes a parameter has a usual value that callers rarely change. Rather than making everyone pass it every time, you can give the parameter a default value in the definition, using an equals sign. If the caller omits that argument, the default is used. If the caller supplies one, theirs wins."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the function and ask for each call before revealing: "What does each call return?" Answers: 42.0, 31.5, 31.5, and 42.0. For the last line, ask which arguments were passed and which used the default.

**Demo 7 — A default parameter value (whiteboard-friendly)**

```python
def line_total(unit_price, quantity, discount=0):
    return unit_price * quantity * (1 - discount)

print(line_total(3.50, 12))                     # 42.0   (default discount of 0)
print(line_total(3.50, 12, 0.25))               # 31.5   (discount given by position)
print(line_total(3.50, 12, discount=0.25))      # 31.5   (discount given by name)
print(line_total(quantity=12, unit_price=3.50)) # 42.0   (keywords, default again)
```

**[Script:]**

"The first call omits the discount, so the default of 0 applies, and nothing is taken off. The second and third supply a discount of 0.25, which takes a quarter off, giving 31.5. Using the keyword form, as in the third call, makes it clear what the 0.25 means. Defaults make the common case short and the unusual case possible."

---

### 4B — One Ordering Rule

**[Script:]**

"There is one rule about ordering. Parameters without defaults must come before parameters with defaults. Python needs to know which arguments are required before it considers which are optional."

> 🎯 **Instructor Note:** Show the invalid definition and the valid one. The exact wording of the SyntaxError varies between Python versions; the meaning is that a parameter without a default follows one with a default. Ask: "Why would Python reject the first version?" Answer: if the discount came first with a default, a call like `line_total(3.50, 12)` would be ambiguous about which parameter each value should fill.

**Demo 8 — Parameter ordering (whiteboard-friendly)**

```python
def line_total(discount=0, unit_price, quantity):    # SyntaxError
    ...

def line_total(unit_price, quantity, discount=0):    # correct
    ...
```

> 🎯 **Instructor Note:** Name the connection to the previous block: everything about positional and keyword arguments still applies. A default simply makes some arguments optional. Mention a design tip: a number that a function depends on, such as a tax rate or a threshold, is often better as a parameter with a default than as a value hard-coded in the body, because callers can change it without editing the function. Do not go further into default values than this.

**Recap of Block 4 before moving on:**

- A default value is given in the definition with `=`, and is used when the caller omits that argument
- A caller can override a default by position or by keyword
- Parameters without defaults must come before parameters with defaults
- Defaults keep the common call short while leaving unusual cases possible, and keyword arguments make overrides clear

---

## Block 5 — Scope: Why a Variable Inside a Function Is Not Visible Outside

### 5A — The Surprise

**[Script:]**

"Here is the rule that surprises almost every beginner. A variable that is created inside a function exists only inside that function. Code outside it cannot see it. Let us see this fail, and then see why it is designed this way."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the demo with the last line covered. Ask: "The function ran, and it computed `subtotal`. What happens when the last line tries to print it?" Answer: a NameError, saying `subtotal` is not defined. Many learners expect it to work because the function ran in the same notebook.

**Demo 9 — A variable that disappears (whiteboard-friendly)**

```python
def total_price(unit_price, quantity):
    subtotal = unit_price * quantity
    return subtotal

print(total_price(3.50, 12))    # 42.0
print(subtotal)                 # NameError: name 'subtotal' is not defined
```

---

### 5B — Why: Each Call Gets Its Own Workspace

**[Script:]**

"Here is what happens. Each time a function is called, Python creates a temporary private workspace for that call. The parameters and any variables you create in the body live in that workspace, and they are called local variables. When the function returns, the workspace is thrown away, and everything in it disappears. Only the returned value comes out.

Think of a workshop with a closed door. You hand in the materials, which are the arguments. Work happens inside. Only the finished product, the return value, is passed out through the door. The scraps stay inside and are cleaned up."

> 🎯 **Instructor Note:** Draw two boxes on the board, one labeled "the notebook" and one labeled "inside total_price", with an arrow carrying the arguments in and another carrying the return value out. Keep it visible through the rest of the block.

**Demo 10 — Tracing a call's workspace (whiteboard-friendly)**

```
Calling total_price(3.50, 12):
  A new workspace is created:   unit_price = 3.5, quantity = 12
  subtotal = 42.0               (created inside the workspace)
  return 42.0                   (the value leaves; the workspace is discarded)
After the call:  unit_price, quantity, and subtotal no longer exist.
```

**[Script:]**

"This is why `print(subtotal)` fails: by the time that line runs, the workspace that held `subtotal` is gone. If you want a value to come out of a function, return it. That is exactly what `return` is for."

---

### 5C — Same Name, Different Variable

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "There is a `total` outside the function and a `total` inside it. What is printed at the end?" Collect answers. The correct answer is `15` and then `100`. Many learners expect the function to have changed the outer `total`.

**Demo 11 — Two variables with the same name (whiteboard-friendly)**

```python
total = 100

def add_fee(price):
    total = price + 5       # a NEW local variable that happens to be called total
    return total

print(add_fee(10))          # 15
print(total)                # 100   (the outer total was never touched)
```

**[Script:]**

"Assigning to `total` inside the function created a brand new local variable, separate from the outer `total`. They share a name but they are different variables in different workspaces. The outer one still holds 100.

This isolation is a feature. It means you can write a function without worrying about what names the rest of the notebook uses, and calling the function cannot accidentally overwrite your other variables. It also makes tracing much easier, because you only need to look at the function's inputs and its return value to know what it does.

A function can read a variable defined outside it, but relying on that makes the function harder to reuse and harder to trace. The better habit is to pass what the function needs in as arguments, using a default value if there is a standard one."

> 🎯 **Instructor Note:** Ask: "If I want the value computed inside the function to be available afterward, what do I do?" Answer: return it, and store the returned value in a variable outside: `result = total_price(3.50, 12)`. Also name the notebook link: variables created at the top level of a notebook cell stay in memory for later cells, but variables created inside a function do not. If a learner asks about the `global` keyword, say that it exists but that changing outside variables from inside a function makes code hard to trace, so it is best avoided for now.

**Recap of Block 5 before moving on:**

- A variable created inside a function is a local variable and exists only in that call's workspace
- Each call creates a fresh workspace, which is discarded when the function returns; only the returned value comes out
- Parameters are local variables too
- A local variable can share a name with an outer variable, but they are different variables, and assigning inside does not change the outer one
- To get a value out of a function, return it; to give a function a value, pass it in as an argument

---

## Block 6 — Reading Classes: Definitions and Instances

### 6A — Why Classes

**[Script:]**

"So far a function bundles up actions. A class bundles up data together with the actions that belong with that data. An order has a customer and a total, and things you might ask of an order, such as whether it is large. A class lets you describe that once.

We focus today on reading classes: looking at a class definition and understanding what it describes and how it is used. You will meet classes all the time in real code. Remember that `type("cat")` reported `str`? `str` is a class, and the text `"cat"` is one object created from it. The same goes for `int` and `float`."

---

### 6B — The Central Distinction

**[Script:]**

"The most important idea in this block is the difference between a class and an instance. A class definition is a blueprint. It describes what data each object will hold and what each object can do. Writing a class definition does not create any objects. An instance is one actual object created from that blueprint, with its own data. You can create as many instances from one class as you like."

> 🎯 **Instructor Note:** Write this on the board and keep it visible for the rest of the block. Use the analogy of a form template and filled-in forms, or a house blueprint and the houses built from it. Ask learners to supply their own analogy to check understanding.

```
CLASS      the blueprint / template        defined once
           describes: what data, what actions

INSTANCE   one object built from the blueprint     created many times
           has its own data

  Order          →  the class
  order_a        →  one instance
  order_b        →  another instance
```

---

### 6C — Reading a Class Definition

**[Script:]**

"Here is a simple class. Read it with me."

**Demo 12 — A class definition (whiteboard-friendly)**

```python
class Order:
    def __init__(self, customer_id, total):
        self.customer_id = customer_id
        self.total = total

    def is_large(self):
        return self.total >= 100
```

**[Script:]**

"`class Order:` begins the definition; by convention, class names start with capital letters. Inside are functions, called methods when they live inside a class.

`__init__` is a special method that runs automatically every time a new instance is created. Its job is to set up the instance's starting data. `self` is the first parameter of every method, and it means 'this particular instance.' Python fills it in automatically; you never pass it yourself. The lines `self.customer_id = customer_id` and `self.total = total` store the values on the instance. These stored values are called attributes.

`is_large` is an ordinary method. It uses `self.total`, so the answer depends on which instance you ask."

> 🎯 **Instructor Note:** Give learners a method for reading any class and write it on the board.

```
Reading a class:
1. The class name: what thing does it describe?
2. __init__: what data does each instance hold? (the self.something lines)
3. The other methods: what can each instance do?
4. Look for lines that create instances: how is the class actually used?
```

---

### 6D — Creating and Using Instances

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** First ask: "After the class definition cell has run, and before any other line, how many Order objects exist?" Answer: none. The definition only describes them. Then show the instance lines with the output covered and ask for each print. Answers: 150.0, 40.0, True, False.

**Demo 13 — Instances from one class (whiteboard-friendly)**

```python
order_a = Order("C-42", 150.00)
order_b = Order("C-77", 40.00)

print(order_a.total)          # 150.0
print(order_b.total)          # 40.0
print(order_a.is_large())     # True
print(order_b.is_large())     # False
```

**[Script:]**

"Calling `Order(...)` creates a new instance. Python runs `__init__` with that instance as `self`, using the arguments we supplied, in order, positionally. `order_a` and `order_b` are two separate instances of the same class. Each holds its own `customer_id` and `total`. You read an attribute with a dot, as in `order_a.total`, and you call a method with a dot and parentheses, as in `order_a.is_large()`.

Everything you learned about calling functions applies: you could write `Order(customer_id="C-42", total=150.00)` with keywords, and `__init__` could have default values for its parameters."

---

### 6E — Instances Have Independent Data

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I change `order_a.total` to 20, what happens to `order_b.total`, and what does `order_a.is_large()` return now?" Answer: `order_b.total` stays 40.0, and `order_a.is_large()` becomes False. Each instance's data is independent, and the method reads the instance's current data.

**Demo 14 — Changing one instance (whiteboard-friendly)**

```python
order_a.total = 20.00
print(order_a.total, order_b.total)    # 20.0 40.0
print(order_a.is_large())              # False

print(type(order_a))                   # <class '__main__.Order'>
print(isinstance(order_a, Order))      # True
```

**[Script:]**

"Changing `order_a` did not touch `order_b`. The method gave a different answer for `order_a` because the data behind it changed. `type(order_a)` shows the class it was built from, and `isinstance` confirms that `order_a` is an instance of `Order`. In a notebook, the class name appears with `__main__` in front of it, which just means it was defined in the notebook itself."

> 🎯 **Instructor Note:** Ask: "What is the difference between `Order` and `order_a`?" Answer: `Order` is the class, the blueprint; `order_a` is one instance, one actual order with its own data. Name the common confusions: learners often think defining the class creates an object, and often think changing one instance changes them all. Ask what `Order.total` would give. It raises an AttributeError, because `total` is stored on each instance when it is created, and the blueprint itself holds no particular total.

---

### 6F — Reading Practice

**[Script:]**

"Now you read. Here is a class that you have not seen. Without running it, tell me what each instance holds, what it can do, and what the output will be."

> 🎯 **Instructor Note:** Run this as a short pair exercise, four to five minutes. Write the class and the usage on the board. Have learners build a small trace table of each instance's `points` as the lines execute, using the tracing method from earlier.

```python
class Customer:
    def __init__(self, name):
        self.name = name
        self.points = 0

    def add_points(self, amount):
        self.points += amount
```

```python
c1 = Customer("Asha")
c2 = Customer("Ben")
c1.add_points(120)
c2.add_points(30)
print(c1.points, c2.points)
```

> 🎯 **Instructor Note:** Answers: each instance holds a `name` and `points`, which starts at 0; each can add points to itself. The output is `120 30`. Build the trace together.

```
Step                      c1.points   c2.points
c1 = Customer("Asha")     0           -
c2 = Customer("Ben")      0           0
c1.add_points(120)        120         0
c2.add_points(30)         120         30
```

**[Script:]**

"Each instance carries its own `points`, starting from the same blueprint and the same starting value of 0. `add_points` changes only the instance it is called on, because `self` refers to that instance. When you meet an unfamiliar class in real code, this is how to read it: find the attributes in `__init__`, find the methods, and look for where instances are created."

**Recap of Block 6 before moving on:**

- A class definition is a blueprint; it describes what data each object holds and what each can do, and creates no objects by itself
- An instance is one object created from the class, with its own data; one class can produce many instances
- `__init__` runs automatically when an instance is created and sets up its attributes; `self` means the particular instance and is supplied by Python
- Attributes are read with a dot, as in `order_a.total`, and methods are called with a dot and parentheses, as in `order_a.is_large()`
- Changing one instance does not change another, and a method's result depends on the instance's current data
- To read an unfamiliar class: find the name, the attributes in `__init__`, the methods, and where instances are created

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as rapid active recall. Ask each question and take answers before confirming. "What is the difference between a parameter and an argument? What is the difference between `return` and `print`? What does a function return if it has no `return` statement? How does `change_due(72.50, 100)` differ from `change_due(total=72.50, paid=100)`? Where must default parameters sit in the parameter list? Why can't code outside a function see a variable created inside it? What is the difference between a class and an instance? What does `self` mean?" Finish by returning to the opening scenario and asking how a function, with a default threshold, solves the five-copy shipping rule problem.

**Writing Functions**

- A function is defined with `def`, a name, parameters, and an indented body, and called by name; defining does not run it
- A parameter is the name in the definition and an argument is the value passed in
- `return` hands a value back; `print` only displays it, and a function with no `return` gives back `None`

**Calling Functions**

- Positional arguments are matched by order, and keyword arguments by name; positional ones must come first
- Missing, extra, or misspelled arguments raise a TypeError

**Default Parameter Values**

- A default is set in the definition with `=` and used when the caller omits the argument; it can be overridden by position or keyword
- Parameters without defaults must come before those with defaults

**Scope**

- A variable created inside a function is local to that call's workspace, which is discarded when the function returns
- A local variable can share a name with an outer variable without affecting it
- To get a value out, return it; to give a function a value, pass it in

**Reading Classes**

- A class definition is a blueprint and an instance is one object built from it, with its own data
- `__init__` sets up each instance's attributes, `self` means the particular instance, and methods are functions inside a class
- Changing one instance does not change another

**Why All of This Matters Together**

- Functions turn a rule written five times into a rule written once, and the pieces you learned today make that safe: parameters and return values give a function a clear way in and a clear way out, keyword and default arguments make calls readable and flexible, and local scope keeps each call isolated so one function cannot accidentally disturb another; classes extend the same idea by bundling data with the actions that belong to it, and the distinction between a blueprint and the objects built from it is what lets you read any class you meet; together, these let you organize code so that a change is made in one place, a result can be traced from its inputs, and other people's code becomes something you can read instead of something you must guess at

---

*End of script.*
