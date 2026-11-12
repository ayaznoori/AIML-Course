# Lecture Script: Loops and Tracing What Runs
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Lecture Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | for Loops Over a Sequence | 17 min |
| 3 | while Loops and Stated Termination Conditions | 17 min |
| 4 | break and continue | 15 min |
| 5 | Predicting Which Branch Executes | 13 min |
| 6 | Tracing Variable Values Through Iterations | 18 min |
| 7 | Finding the Condition That Prevents Termination | 15 min |
| 8 | Lecture Summary and Recap | 7 min |

---

## Learning Objectives

By the end of this lecture, learners will be able to:

1. Write a for loop that iterates over a sequence
2. Write a while loop that terminates on a stated condition
3. Apply break and continue to exit or skip iterations in a loop
4. Predict which branch of a conditional executes for a given input
5. Trace the value of a variable through successive loop iterations
6. Identify the condition in a loop that prevents it from terminating

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience already knows variables, the four basic types, arithmetic including `//` and `%`, boolean expressions, and if / elif / else, and works in Google Colab notebooks. Loops are the first place where a program does something many times, which means the first place where a mistake can repeat many times too. The hook should present three realistic failures and name the one skill that diagnoses all of them: knowing exactly which lines run, in what order, and with what values. Open with the question and wait for answers before continuing.

**[Script:]**

"Here is a question to start. You run a cell in your notebook. Ten minutes later it is still running, there is no output, and the indicator is still spinning. What is the most likely thing that happened? Take a moment and give me a guess.

Most likely, a loop never stopped. The program is doing exactly what you told it to do, forever. That is the first failure.

Here are two more. A program is meant to add up five prices, and it reports a total that is missing the last one. Nothing crashed and no error message appeared; the loop simply ran four times instead of five. And a pricing rule never seems to trigger, no matter what input you feed it, because the branch that should handle it can never be reached.

None of these failures produce a helpful error message. The program runs, and it does the wrong thing. The only way to find the problem is to read the code the way Python reads it: one line at a time, in order, keeping track of what every variable holds at every moment. That skill is called tracing, and it is the real subject of today's lecture.

We will learn the two kinds of loop, the for loop, which goes through a collection of things, and the while loop, which repeats until a condition changes. We will learn break and continue, which let you exit a loop early or skip one pass. Then we will practice the skill that ties it together: predicting which branch of a conditional runs for a given input, following a variable's value through each pass of a loop, and finding the exact condition that stops a loop from ever finishing."

---

## Block 2 — for Loops Over a Sequence

### 2A — Repeating an Action for Each Item

**[Script:]**

"Imagine you have five prices and you want to print each one. You could write five print statements. With five hundred prices, that approach falls apart. A loop lets you write the action once and have Python repeat it.

A for loop works with a sequence, an ordered collection of items that can be gone through one at a time. Three sequences matter today. A list is an ordered collection written in square brackets, such as `[3.50, 0.75, 24.00]`. A string is a sequence of characters. And `range()` produces a sequence of whole numbers."

> 🎯 **Instructor Note:** Write the structure on the board and keep it visible for the whole lecture.

```
for item in sequence:
    # the body: runs once for each item in the sequence

- "item" is the loop variable: it takes the first value, then the second, ...
- the colon ends the for line; the body is indented
- the loop ends when the sequence runs out
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the list loop with the output covered. Ask: "How many lines will this print, and what will each one be?" Answer: three lines, 3.5, 0.75, and 24.0. Point out that the loop variable `price` holds the value itself, not its position in the list.

**Demo 1 — Looping over a list and over text (whiteboard-friendly)**

```python
prices = [3.50, 0.75, 24.00]
for price in prices:
    print(price)
# 3.5
# 0.75
# 24.0

for ch in "cat":
    print(ch)
# c
# a
# t
```

**[Script:]**

"On the first pass, `price` is 3.5. The body runs. On the second pass, `price` is 0.75, and the body runs again. After the third, the list is exhausted and the loop ends. A string works the same way, with each character taken in turn."

---

### 2B — range(): A Sequence of Numbers

**[Script:]**

"When you want to repeat something a certain number of times, or count, you use `range()`. It produces whole numbers. With one number, `range(5)` starts at 0 and goes up to, but does not include, 5. With two numbers, the first is the start and the second is the stop. With three, the third is the step."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What numbers does `range(1, 4)` produce? How many numbers is that?" Answer: 1, 2, 3, which is three numbers. Name the first common confusion: the stop value is not included. Give the rule: the count of numbers is the stop minus the start. This is the mistake from the opening: `range(1, 5)` for five items would produce only four.

**Demo 2 — What range produces (whiteboard-friendly)**

```python
# list() is used here only so we can see the numbers range produces
print(list(range(5)))          # [0, 1, 2, 3, 4]
print(list(range(2, 6)))       # [2, 3, 4, 5]
print(list(range(0, 10, 3)))   # [0, 3, 6, 9]

for i in range(3):
    print("Pass", i)           # Pass 0, Pass 1, Pass 2
```

**[Script:]**

"`range(5)` gives five numbers, starting at 0, so the last one is 4. `range(2, 6)` starts at 2 and stops before 6. `range(0, 10, 3)` steps by 3. The stop value is always excluded. If you want to run exactly five times, `range(5)` does it. If you want the numbers 1 through 5, you need `range(1, 6)`."

---

### 2C — Accumulating a Result

**[Script:]**

"The most common thing a for loop does is build up a result. You create a variable before the loop, usually starting at 0, and add to it on every pass. This is called an accumulator."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the code with the final comment covered. Ask: "What does this print?" Answer: 28.25. Then ask: "What if `total = 0` were moved inside the loop, as the first line of the body?" Answer: it would reset to 0 on every pass, so the final total would be 24.0, just the last price. The accumulator must be created before the loop.

**Demo 3 — The accumulator pattern (whiteboard-friendly)**

```python
prices = [3.50, 0.75, 24.00]
total = 0
for price in prices:
    total = total + price
print(total)       # 28.25
```

**[Script:]**

"Before the loop, `total` is 0. After the first pass it is 3.5, after the second 4.25, after the third 28.25. The print is outside the loop, so it runs once, after the loop is finished. How do we know it is outside? Indentation. Python uses indentation to decide which lines belong to the loop body."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show Demo 4 and ask: "How many times does the text 'after loop' print?" Answer: once. Name the confusion: the line not indented under the `for` belongs after the loop and runs one time. If a learner indents it, it runs on every pass. Mention that Python raises an `IndentationError` if the body of a loop is not indented at all.

**Demo 4 — Indentation decides what repeats (whiteboard-friendly)**

```python
for n in [1, 2, 3]:
    print("in loop:", n)
print("after loop")
# in loop: 1
# in loop: 2
# in loop: 3
# after loop
```

**Recap of Block 2 before moving on:**

- A for loop runs its indented body once for each item in a sequence, with the loop variable taking each value in turn
- Sequences include lists, strings, and the numbers produced by `range()`
- `range(stop)` starts at 0, `range(start, stop)` begins at start, and `range(start, stop, step)` counts by step; the stop value is never included
- An accumulator is created before the loop and updated inside it
- Indentation decides which lines repeat and which run once after the loop

---

## Block 3 — while Loops and Stated Termination Conditions

### 3A — Repeating Until a Condition Changes

**[Script:]**

"A for loop is right when you have a sequence or a known number of repetitions. Sometimes you do not know in advance how many passes you need. You want to keep going until something becomes true. For that, Python has the while loop. It checks a boolean expression, the same kind you built with comparison operators, and runs its body as long as the expression is True."

> 🎯 **Instructor Note:** Write the three requirements on the board and keep them visible through Block 7. They are the diagnostic checklist for everything about termination.

```
while condition:
    # body

Every while loop needs three things:
1. A starting value, set BEFORE the loop
2. A condition, checked before EACH pass
3. An update INSIDE the loop that moves toward making the condition False
```

**[Script:]**

"The condition is checked before every pass, including the first. If it is False at the start, the body never runs at all. And here is a subtle point that matters in practice. A business rule is usually stated as when to stop: 'stop when the balance reaches 1,000.' But a while condition says when to continue. So you must write the opposite of the stop rule. Stop when the balance is at least 1,000 means continue while the balance is less than 1,000."

---

### 3B — Writing a Loop From a Stated Condition

**[Script:]**

"Here is a stated rule. A savings plan adds 150 to a balance each month, starting at 0. Count how many months are needed until the balance reaches at least 1,000."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask the room to work it out first: "After 6 months the balance is 900. Is that enough? How many months, and what is the final balance?" Answer: 900 is not enough, so 7 months, and the final balance is 1,050. Ask which condition keeps the loop going: `balance < 1000`. Ask what would go wrong with `balance <= 1000`: nothing here, but it would run one extra pass if the balance landed exactly on 1,000, so match the stated rule carefully.

**Demo 5 — A while loop from a stated stop rule (whiteboard-friendly)**

```python
balance = 0
months = 0
while balance < 1000:
    balance += 150
    months += 1
print(months, balance)     # 7 1050
```

**[Script:]**

"The starting values are set before the loop. The condition `balance < 1000` is the opposite of the stop rule. Inside the loop, `balance` grows, which moves toward making the condition False, and `months` counts the passes. When the balance reaches 1,050, the condition is False, the loop ends, and the print runs once.

Notice we could not have used a for loop so neatly, because we did not know the number of months in advance. The condition decided."

> 🎯 **Instructor Note:** Put the choice rule on the board and ask learners to apply it to two cases: "Add up every price in a list" and "Keep asking until the user types a valid answer." Answers: for loop, because the sequence is known; while loop, because the number of repetitions depends on a condition.

```
Use for    when you have a sequence or a known number of repetitions
Use while  when you repeat until a condition changes and
           do not know in advance how many passes it will take
```

**Recap of Block 3 before moving on:**

- A while loop repeats its body as long as its condition is True, checking before every pass, including the first
- A while loop needs a starting value, a condition, and an update that moves toward making the condition False
- A stop rule and a loop condition are opposites: stop when the balance reaches 1,000 means continue while it is below 1,000
- Use a for loop for a known sequence or number of passes, and a while loop when a condition decides when to stop

---

## Block 4 — break and continue

### 4A — Exiting or Skipping

**[Script:]**

"Sometimes you need finer control inside a loop. Two keywords give it to you. `break` exits the loop immediately, and the program continues with the first line after the loop. `continue` skips the rest of the current pass and jumps to the next one. The loop itself keeps going."

> 🎯 **Instructor Note:** Write this on the board. The difference between ending the loop and skipping one pass is the whole idea, so use those exact words.

```
break      → leave the loop entirely, right now
continue   → skip the rest of THIS pass, go on to the next pass
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show both loops with the output covered. Ask: "The two loops are identical except for one keyword. What does each one print?" Answers: with `break`, 1 and 2, because the loop ends when `n` is 3. With `continue`, 1, 2, 4, and 5, because only the pass where `n` is 3 is skipped.

**Demo 6 — break compared with continue (whiteboard-friendly)**

```python
for n in range(1, 6):
    if n == 3:
        break
    print(n)             # 1  2

for n in range(1, 6):
    if n == 3:
        continue
    print(n)             # 1  2  4  5
```

**[Script:]**

"Both loops reach the passes where `n` is 1 and 2 and print them. At `n == 3`, `break` ends the whole loop, so 4 and 5 never appear. `continue` skips only the print for 3, and the loop carries on with 4 and 5."

---

### 4B — Practical Uses

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the break search below and ask: "Which prices are printed? Does the loop ever look at 1.2 or 30.0?" Answer: only 24.0 is printed, and the loop never looks at 1.2 or 30.0, because `break` ends the loop at the first match. Ask what the loop would do without the break: it would report every price over 20.

**Demo 7 — Using break to stop at the first match (whiteboard-friendly)**

```python
prices = [3.5, 0.75, 24.0, 1.2, 30.0]
for price in prices:
    if price > 20:
        print("First expensive item:", price)
        break
# First expensive item: 24.0
```

**[Script:]**

"`break` is for when you have found what you were looking for, and continuing would be wasted work or would give a wrong answer. Here we only wanted the first expensive item.

`continue` is for when some items should simply be ignored. Suppose a sensor reports -1 when it has no reading, and you want to add up only the valid ones."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What is the final value of `total`, and how many valid readings were counted?" Answer: 13 and 3. The two -1 values are skipped before reaching the lines that add and count.

**Demo 8 — Using continue to skip invalid values (whiteboard-friendly)**

```python
readings = [4, -1, 7, -1, 2]
total = 0
valid = 0
for r in readings:
    if r < 0:
        continue
    total += r
    valid += 1
print(total, valid)      # 13 3
```

**[Script:]**

"When `r` is -1, the condition is True, `continue` runs, and Python jumps straight to the next reading without executing `total += r` or `valid += 1`. Only the lines before `continue` run on a skipped pass."

> 🎯 **Instructor Note:** Name the confusion directly: `break` and `continue` look similar but do different jobs. Ask each learner to say in one sentence the difference. Also mention that both apply to the loop they are written inside. Mention that `break` also works in a while loop, and that a loop written as `while True:` runs until a `break` inside it is reached, so there must be a `break` that can actually run. Do not go deeper unless asked.

**Recap of Block 4 before moving on:**

- `break` exits the loop immediately, and the program continues after the loop
- `continue` skips the rest of the current pass and moves on to the next pass
- `break` suits finding the first match; `continue` suits ignoring values that should not be processed
- Only the lines before `continue` run on a skipped pass

---

## Block 5 — Predicting Which Branch Executes

### 5A — A Method for Predicting

**[Script:]**

"You already know that if, elif, and else give a program branches. The skill we practice now is predicting which branch runs for a particular input, without running the code. This matters inside loops, where the same conditional runs many times with different values, and a different branch may run on each pass.

Use the same method every time."

> 🎯 **Instructor Note:** Write the method on the board and keep it visible for the rest of the lecture.

```
To predict which branch runs for an input:
1. Substitute the input value into the FIRST condition. True or False?
2. If True, that branch runs and ALL the others are skipped. Stop.
3. If False, move to the next condition and repeat.
4. If every condition is False, the else branch runs (if there is one).
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the loop with the output covered. Ask for each temperature which branch runs before revealing: 12 runs `else`, 25 runs the `elif`, 31 runs the `if`. Then ask about the boundary values 30 and 20: 30 runs the `if` because the rule uses `>=`, and 20 runs the `elif`.

**Demo 9 — A conditional inside a loop (whiteboard-friendly)**

```python
for temp in [12, 25, 31]:
    if temp >= 30:
        label = "hot"
    elif temp >= 20:
        label = "warm"
    else:
        label = "cool"
    print(temp, label)
# 12 cool
# 25 warm
# 31 hot
```

**[Script:]**

"For 12: is 12 at least 30? No. At least 20? No. So the else runs. For 25: not at least 30, but at least 20, so the elif runs and the else is skipped. For 31: at least 30, so the first branch runs and the other two are never even checked."

---

### 5B — Practice

**[Script:]**

"Now you predict. Here is a grading rule. For each input, name the branch that runs: A, B, C, or D. Work in pairs, and write your answers before we compare."

> 🎯 **Instructor Note:** Run this as a short pair exercise, three to four minutes. Write the code and the inputs on the board.

```
if score >= 90:       # branch A
elif score >= 75:     # branch B
elif score >= 60:     # branch C
else:                 # branch D

Inputs: 90, 89, 75, 60, 59, 100
```

> 🎯 **Instructor Note:** Answers in order: A, B, B, C, D, A. Spend time on the boundaries: 90 runs A because `>=` includes 90, 89 fails the first test and runs B, 75 runs B, 60 runs C, and 59 fails every condition and runs D. 100 satisfies the first condition, so A runs. Ask a pair to state out loud why 100 does not run B even though 100 is at least 75: the first True branch wins and the rest are skipped.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show this second case and ask: "For x equal to 25, which branch runs? Is there any input for which branch B runs?" Answer: for 25, branch A runs. Branch B can never run, for any input, because any number above 20 is already above 10 and is caught by the first condition. This is the unreachable branch from the opening. Say the fix: put the more restrictive condition first.

**Demo 10 — A branch that can never run (whiteboard-friendly)**

```python
if x > 10:
    print("A")
elif x > 20:
    print("B")      # never runs for any x
```

**Recap of Block 5 before moving on:**

- To predict a branch, test the conditions from the top down, and the first True condition's branch runs
- All branches after the first True one are skipped, and else runs only when every condition is False
- Inside a loop, the same conditional is evaluated again on every pass, so different passes can run different branches
- Boundary values, such as 90 with `>=`, deserve special attention
- If an earlier condition already catches everything a later one would, the later branch can never run

---

## Block 6 — Tracing Variable Values Through Iterations

### 6A — The Trace Table

**[Script:]**

"Now we combine everything into the central skill: tracing. A trace table records the value of each variable as the program runs. You go through the code one pass at a time, exactly as Python would, and write down what changes. Use one row per pass of the loop and one column for each variable that matters. Start with a row for the values before the loop begins."

> 🎯 **Instructor Note:** Write the method on the board. Insist that learners write on paper or on the board while tracing, rather than computing in their heads. Most tracing errors come from skipping rows.

```
Tracing a loop:
1. Write down the starting values of every variable (the row before the loop)
2. For each pass: note the loop variable (for) or check the condition (while)
3. Run each line of the body in order and update the values
4. Write the values at the END of the pass as one row
5. Stop when the sequence ends or the condition is False
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before tracing, ask: "What is the final value of `total`?" Collect guesses, then build the table with the room, one row at a time, asking learners to call out each new value before you write it. The answer is 26. Compare it with the guesses afterward.

**Demo 11 — Tracing a for loop (whiteboard-friendly)**

```python
total = 0
for n in [4, 7, 2]:
    total = total + n * 2
print(total)

Pass   n    n * 2   total after pass
start  -    -       0
1      4    8       8
2      7    14      22
3      2    4       26

Output: 26
```

**[Script:]**

"Before the loop, `total` is 0. On the first pass `n` is 4, and `n * 2` is 8, so `total` becomes 0 plus 8. On the second pass it grows by 14, and on the third by 4. The print runs once, after the loop, and shows 26. Notice we never computed anything in our heads that we could not see in the table."

---

### 6B — Tracing a while Loop

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "How many passes does this loop make, and what are `x` and `steps` at the end?" Collect guesses and then trace together. The answer is 3 passes, `x` equal to 2, and `steps` equal to 3. Point out the extra column in a while trace: the condition result, checked before each pass, including the final check that ends the loop.

**Demo 12 — Tracing a while loop (whiteboard-friendly)**

```python
x = 20
steps = 0
while x > 3:
    x = x // 2
    steps += 1

Check        x   steps   Result
start        20  0
x > 3 ?      20  0       True → run body
after pass 1 10  1
x > 3 ?      10  1       True → run body
after pass 2 5   2
x > 3 ?      5   2       True → run body
after pass 3 2   3
x > 3 ?      2   3       False → loop ends
```

**[Script:]**

"The condition is checked before every pass. Here it is True for 20, 10, and 5, so three passes happen. After the third pass `x` is 5 // 2, which is 2. The next check, 2 greater than 3, is False, and the loop ends. Final values: `x` is 2 and `steps` is 3."

---

### 6C — Tracing Everything Together

**[Script:]**

"Here is a loop that uses everything from today: a for loop, a conditional, continue, and break. You will trace it yourselves. Take two minutes, and write your own table."

> 🎯 **Instructor Note:** Give learners two to three minutes to build the table on paper before revealing anything. Circulate. Then build the table together on the board, asking different learners to call out each row. The final value of `count` is 9.

**Demo 13 — A capstone trace (whiteboard-friendly)**

```python
count = 0
for n in range(1, 8):
    if n % 2 == 0:
        continue
    if n > 5:
        break
    count += n
print(count)

n   n % 2 == 0?   n > 5?   action         count after
1   no            no       count += 1     1
2   yes           -        continue       1
3   no            no       count += 3     4
4   yes           -        continue       4
5   no            no       count += 5     9
6   yes           -        continue       9
7   no            yes      break          9

Output: 9
```

**[Script:]**

"Even values of `n` are skipped by continue, because `n % 2` is 0 for even numbers. Odd values are added, until `n` reaches 7. At 7, the second condition is True, and `break` ends the loop before `count += n` runs. So 7 is never added. The total is 1 plus 3 plus 5, which is 9. Notice that the loop was written to run through 7, but it stopped when n was 7. Tracing is how you know that."

> 🎯 **Instructor Note:** Ask: "What would the answer be if `break` were replaced with `continue`?" Answer: 9 as well, because the number 7 would be skipped either way, but the loop would then end naturally after `n` is 7 since range(1, 8) stops there. This is a useful moment to stress that two different programs can give the same output, and tracing shows you which lines actually ran. Ask also: "How would the trace change if `count = 0` were inside the loop?" Answer: it would reset on every pass, and the printed result would be 0 after the loop ends, since the last executed pass is reset first and nothing is added at 7.

**Recap of Block 6 before moving on:**

- A trace table has one column per variable and one row per pass, starting with a row for the values before the loop
- For a for loop, record the loop variable on each pass; for a while loop, also record the result of the condition check
- Update values line by line in the order Python runs them, and write each row at the end of the pass
- A while loop's final check, the one that is False, is part of the trace
- Writing the table on paper or on the board prevents the skipped rows that cause most tracing errors

---

## Block 7 — Finding the Condition That Prevents Termination

### 7A — Why a Loop Does Not Stop

**[Script:]**

"Return to the first failure: the cell that never finishes. A while loop that never terminates is called an infinite loop. It is not a Python bug. It is a loop whose condition never becomes False. To find the problem, you do not need to run it. You look at the condition and ask what changes."

> 🎯 **Instructor Note:** Write this diagnostic checklist on the board and keep it visible through the end of the lecture.

```
Why is this loop not stopping?
1. Which variables appear in the condition?
2. Does anything inside the loop change them?
3. Does the change move toward making the condition False?
4. Can anything (such as continue) skip the change?
5. Can the value jump over the target, so a == or != test is never met?
```

> 🎯 **Instructor Note:** A practical safety note for the room. Do not have learners run the broken examples in Colab unless they know how to stop a cell. In Colab, the play button on a running cell becomes a stop button; clicking it interrupts the cell, and the Runtime menu also has an option to interrupt execution. We will trace the broken examples on the board rather than running them.

---

### 7B — Four Broken Loops

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the first pair and ask: "In each loop, which variable is in the condition, and what changes it?" Let learners apply the checklist. In the first, nothing changes `count`, so the condition `count < 5` stays True forever. In the second, `n` changes, but in the wrong direction: it grows, and `n > 0` stays True forever.

**Demo 14 — Two causes: no update, and the wrong direction (whiteboard-friendly)**

```python
count = 0
while count < 5:
    print(count)        # BUG: count never changes
                        # Fix: add  count += 1  inside the loop

balance = 1000
years = 0
while balance < 2000:
    balance = balance * 0.95   # BUG: balance shrinks, never reaches 2000
    years += 1                 # Fix: multiply by 1.05; ends with years = 15
```

**[Script:]**

"The first loop has a starting value and a condition but no update. Trace it: `count` is 0, the condition is True, the body prints, and `count` is still 0. The next check is identical, and so is every one after it. The second loop does update the variable, but in the wrong direction: 1,000 times 0.95 is 950, then 902.5, and the balance keeps falling, moving away from the 2,000 it needs to reach. With the fix of 1.05, the balance grows and the loop ends after 15 years."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the next pair. Ask: "In the first, the update exists. What prevents it from running? In the second, the value changes toward 10. So why is there a problem?" Answers: in the first, when `i` is 2, `continue` jumps back to the condition before `i += 1` runs, so `i` stays 2 forever. In the second, the values are 0, 3, 6, 9, 12, and so on, which skip 10 entirely, so `x != 10` is never False.

**Demo 15 — Two more causes: a skipped update, and jumping over the target (whiteboard-friendly)**

```python
i = 0
while i < 5:
    if i == 2:
        continue        # BUG: skips i += 1, so i stays 2
    i += 1
# Fix: put  i += 1  before the if, then continue

x = 0
while x != 10:          # BUG: x goes 0, 3, 6, 9, 12 ... never equals 10
    x += 3
# Fix: while x < 10
```

**[Script:]**

"In the first loop, the update exists but a `continue` can skip it. The traces for the first three passes are `i` equal to 0, then 1, then 2, and from there it stays at 2. In the second, the update exists and moves toward 10, but the loop tests for exact equality. The values step over 10. Using `<` instead of `!=` is the safer way to write a stop rule, because it works even when a value jumps past the limit."

> 🎯 **Instructor Note:** Ask: "Which of the four causes would be caught by simply printing the loop variable inside the loop?" Answer: all four. Printing `count`, `balance`, `i`, or `x` on each pass would show immediately that it never reaches its target. Recommend this as the first debugging step. Also point out that a loop written as `while True:` terminates only if a `break` inside it can actually be reached, which is the same question in a different form.

---

### 7C — Practice

**[Script:]**

"Last exercise. Here is a loop with a problem. Find the condition and the update, explain why it does not terminate, fix it, and predict the output of your fixed version."

> 🎯 **Instructor Note:** Run this as a two-minute pair exercise, then discuss. Write the code on the board.

```python
queue = 10
served = 0
while queue != 0:
    queue -= 3
    served += 1
print(served)
```

> 🎯 **Instructor Note:** Answer: the condition `queue != 0` never becomes False, because `queue` goes 10, 7, 4, 1, -2, -5, and so on, jumping over 0. This is the stepping-over cause. The fix is `while queue > 0`. With the fix, the passes leave `queue` at 7, 4, 1, and -2, so the loop runs 4 times and prints 4. The loop has served 4 groups even though the last group was smaller than 3. Ask learners whether the answer matches the business meaning, and discuss that a different stated rule might call for a different condition.

**Recap of Block 7 before moving on:**

- A loop that never terminates has a condition that never becomes False
- Diagnose it without running it: find the variables in the condition, and check whether the loop changes them, in the right direction, on every pass
- Common causes: no update, an update in the wrong direction, an update skipped by `continue`, and an exact `==` or `!=` test that the value steps over
- Prefer `<` or `>` in a stop rule over exact equality when values change in steps
- Printing the variable in the condition on each pass is the fastest way to see the problem

---

## Block 8 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as rapid active recall. Ask each question and take answers before confirming. "What numbers does `range(2, 6)` produce? When do you use a while loop instead of a for loop? What is the difference between `break` and `continue`? For `score = 75` in the grading rule, which branch runs, and why not the ones below it? In a trace table, what do you record in each row? What are the three things a while loop needs? Name two reasons a loop might never terminate." Return to the three opening failures and ask learners to explain each one: the missing last price (a `range` stop value), the branch that never runs (an earlier condition catching everything), and the cell that never finishes (a condition that cannot become False).

**for Loops Over a Sequence**

- A for loop runs its indented body once per item in a sequence: a list, a string, or `range()`
- `range()` never includes its stop value; an accumulator is created before the loop
- Indentation decides what repeats and what runs once after the loop

**while Loops and Stated Termination Conditions**

- A while loop needs a starting value, a condition checked before each pass, and an update that moves toward False
- A stop rule and a loop condition are opposites
- Use for with a known sequence or count; use while when a condition decides when to stop

**break and continue**

- `break` leaves the loop entirely; `continue` skips only the rest of the current pass

**Predicting Which Branch Executes**

- Test conditions top to bottom; the first True branch runs and the rest are skipped; else runs when all are False
- A later branch can be unreachable if an earlier condition already catches everything it would

**Tracing Variable Values**

- A trace table has a row per pass and a column per variable, starting with the values before the loop
- For a while loop, record each condition check, including the final False one

**Finding the Condition That Prevents Termination**

- Find the variables in the condition and check whether the loop changes them, in the right direction, and whether anything skips the change
- Exact equality tests can be stepped over; `<` and `>` are safer stop rules

**Why All of This Matters Together**

- A loop repeats whatever it contains, so a small mistake becomes a large one: an off-by-one `range`, a branch that cannot run, or a condition that cannot become False; none of these produce a helpful error message, and all of them are found the same way, by reading the code the way Python does, one line at a time, and tracking exactly which lines run and what every variable holds at each step; once you can trace, you can predict a program's behavior before you run it and locate a problem when it behaves differently from what you expected, and that is the foundation for debugging everything more complex you will write

---

*End of script.*
