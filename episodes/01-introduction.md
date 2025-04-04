---
title: Python Basics Recap
teaching: 0
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Understanding key concepts in Python

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Python Refresher

::::::::::::::::::::::::::::::::::::::::::::::::::

This course follows on from the python introduction course. To ensure that we are starting from similar positions, there follows a short multiple choice quiz on key python concepts that we will be building on through this course.

## Variables and Lists

::::::::::::::::::::::::::::::::::::: challenge

### 1\. Assigning a value to a variable

We wish to store the string `Cat` as a value in the variable `animal`, which of these lines of code will do this for us?

1. `animal = 'Cat'`
2. `animal = Cat`
3. `Cat = animal`
4. `animal(Cat)`

:::::::::::::::  solution

Answer 1 is correct

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### 2\. Assigning values to a list

We wish to create a list of values, which of these lines of code is valid to do this?

1. `varlist = [34, 57, '2d']`
2. `varlist = (12, 'vr', 95)`
3. `varlist = 'xcf', 12, 97`

:::::::::::::::  solution

Answer 1 is correct

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### 3a. Indexing characters in a string

Lists and strings both contain multiple indexed values (in the case of strings these are specifically individual characters rather than other values). If we have a variable `animal` which contains the string `penguin`, which of these options will print the first character (`p`) for us?

1. `print(animal[0])`
2. `print(animal[1])`
3. `print(animal['p'])`

:::::::::::::::  solution

Answer 1 is correct

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### 3b. Indexing characters in a string (slicing)

We can also select whole sections of lists and strings, not just single elements. Using the same variable `animal`, containing the string `penguin`, as above, which of these options will print the last 2 characters for us? (Note that there is more an one correct answer)

1. `print(animal[5:])`
2. `print(animal[6:])`
3. `print(animal[6:7])`
4. `print(animal[5:7])`
5. `print(animal[-2:])`
6. `print(animal[:-2])`

:::::::::::::::  solution

Answers 1, 4, and 5 are correct

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::

## Loops

::::::::::::::::::::::::::::::::::::: challenge

### 4\. Constructing a `for` loop

Please write a simple `for` loop which will print out each of the characters in the `animal` variable one at a time.

:::::::::::::::  solution

```python
for char in animal:
    print(char)
```

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::

## Software Modules

::::::::::::::::::::::::::::::::::::: challenge

### 5\. Loading new functions

We want to use the functions in the `numpy` library in our code. How do we open this library in our code?

1. `import numpy`
2. `load numpy`
3. `open numpy`

:::::::::::::::  solution

Answer 1 is correct

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::

## If statements and conditionals

::::::::::::::::::::::::::::::::::::: challenge

### 6\. Conditionals

Which of these conditional tests returns a `True` result?

1. `4 > 3`
2. `'a' != 'b'`
3. `6 >= 6.0`
4. `'3' == 3`
5. `3 > 'c'`

:::::::::::::::  solution

Answers 1, 2, and 3 return `True` results. 4 returns `False`. 5 raises an Error.

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

### 7\. If statements

What is printed when this `if` statement is used?

```python
if 4 > 5:
    print('A')
elif 4 <= 5:
    print('B')
elif 4 < 5:
    print('C')
else:
    print('D')
```

1. `A`
2. `B`
3. `C`
4. `B` and `C`
5. `D`
6. `A` and `D`

:::::::::::::::  solution

`B`. Both `4 <= 5` and `4 < 5` would return a `True` result, but the `if` statement is exited as soon as the `True` result is returned.

:::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::


:::::::::::::::::::::::::::::::::::::::: keypoints

- variables
- lists
- indexing
- loops
- conditionals

::::::::::::::::::::::::::::::::::::::::::::::::::


