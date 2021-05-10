---
title: "Python Basics Recap"
teaching: 0
exercises: 0
questions:
- "Python Refresher"
objectives:
- "Understanding key concepts in Python"
keypoints:
- "variables"
- "lists"
- "indexing"
- "loops"
- "conditionals"
---

This course follows on from the python introduction course. To ensure that we are starting from similar positions, there follows a short multiple choice quiz on key python concepts that we will be building on through this course.

## Variables and Lists

### 1. Assigning a value to a variable
We wish to store the string `Cat` as a value in the variable `animal`, which of these lines of code will do this for us?
1. `animal = 'Cat'`
2. `animal = Cat`
3. `animal = Cat`
4. `animal(Cat)`

> ## Solution
> Answer 1 is correct
{: .solution}

### 2. Assigning values to a list
We wish to create a list of values, which of these lines of code is valid to do this?
1. `varlist = [34, 57, '2d']`
2. `varlist = (12, 'vr', 95)`
3. `varlist = 'xcf', 12, 97`

> ## Solution
> Answer 1 is correct
{: .solution}

### 3a. Indexing characters in a string
Lists and strings both contain multiple indexed values (in the case of strings these are specifically individual characters rather than other values). If we have a variable `animal` which contains the string `penguin`, which of these options will print the first character (`p`) for us?
1. `print(animal[0])`
2. `print(animal[1])`
2. `print(animal['p'])`

> ## Solution
> Answer 1 is correct
{: .solution}


### 3b. Indexing characters in a string (slicing)
We can also select whole sections of lists and strings, not just single elements. Using the same variable `animal`, containing the string `penguin`, as above, which of these options will print the last 2 characters for us? (Note that there is more an one correct answer)
1. `print(animal[3:])`
2. `print(animal[4:])`
2. `print(animal[4:5])`
3. `print(animal[3:5])`
4. `print(animal[-2:])`
5. `print(animal[:-2])`

> ## Solution
> Answers 1, 3, and 4 are correct
{: .solution}


## Loops

### 4. Constructing a `for` loop
Please write a simple `for` loop which will print out each of the characters in the `animal` variable one at a time.

> ## Solution
> ~~~
> for char in animal:
>     print(char)
> ~~~
> {: .language-bash}
{: .solution}

## Software Modules

### 5. Loading new functions
We want to use the functions in the `numpy` library in our code. How do we open this library in our code?
1. `import numpy`
2. `load numpy`
3. `open numpy`

> ## Solution
> Answer 1 is correct
{: .solution}

## If statements and conditionals

### 6. Conditionals
Which of these conditional tests returns a `True` result?
1. `4 > 3`
2. `'a' != 'b'`
3. `6 >= 6.0`
4. `'3' == 3`
5. `3 > 'c'`

> ## Solution
> Answers 1, 2, and 3 return `True` results. 4 returns `False`. 5 raises an Error.
{: .solution}

### 7. If statements
What is printed when this `if` statement is used?
~~~
if 4 > 5:
    print('A')
elif 4 <= 5:
    print('B')
elif 4 < 5:
    print('C')
else:
    print('D')
~~~
{: .language-python}
1. `A`
2. `B`
3. `C`
4. `B` and `C`
5. `D`
6. `A` and `D`

> ## Solution
> `B`. Both `4 <= 5` and `4 < 5` would return a `True` result, but the `if` statement is exited as soon as the `True` result is returned.
{: .solution}

{% include links.md %}

