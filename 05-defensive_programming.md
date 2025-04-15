---
title: Defensive Programming
teaching: 0
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Learn how to catch and deal with errors within your code.
- Learn how to catch problematic data before it is used in your code.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I catch errors in my data?
- How can I make best use of error messages?

::::::::::::::::::::::::::::::::::::::::::::::::::

In this episode we are going to learn about error handling, a python structure (also common in other programming languages) which will help us to properly manage the anomalies that can be encountered when executing our code.

In science most developers write code for themselves and do not feel the need to use error handling. However, nowadays scientists have to share their work as more funding agencies are also asking for the code used in experiments to be published.

More importantly, applications are not expected to crash and we are going to learn how to avoid this non desirable behaviour.

:::::::::::::::::::::::::::::::::::::::  challenge

## What is the problem with this test?

Please look at the following code. Can you find the fundamental problem in this test?

```bash
val = 1

if val>0 and val<10:
    print('Value: ', val, 'is a digit.')
elif val==0:
    print('Value ', val, 'is nul')
else:
    print('Value: ', val, 'is a number.')
```

:::::::::::::::  solution

## Solution

The test assumes that `val` is a number, and throws an uncontrolled error if it is not.

```bash
val = 'a'

if val>0 and val<10:
    print('Value: ', val, 'is a digit.')
elif val==0:
    print('Value ', val, 'is nul')
else:
    print('Value: ', val, 'is a number.')
```

```output
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-2-99c0e25bf5e9> in <module>()
      1 val = 'a'
      2
----> 3 if val>0 and val<10:
      4     print('Value: ', val, 'is a digit.')
      5 elif val==0:

TypeError: '>' not supported between instances of 'str' and 'int'
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

We can avoid problems like this by wrapping our code in an `if` statement:

```python
if type(val) is int or type(val) is float:
    if val>0 and val<10:
        print('Value: ', val, 'is a digit.')
    elif val==0:
        print('Value ', val, 'is nul')
    else:
        print('Value: ', val, 'is a number.')
else:
    print('val is not a number')
```

However, this becomes tedious to implement very quickly, especially when trying to cover a number of possible errors, which will all need their own `if` clauses. This can lead to a lot of duplicated code, making the code difficult to maintain.

Python provides the `try-except` structure to avoid this issue, enabling developers to properly manage any possible errors. The basic `try-except` structure is:

```python
try:
    if val>0 and val<10:
        print('Value: ', val, 'is a digit.')
    elif val==0:
        print('Value ', val, 'is nul')
    else:
        print('Value: ', val, 'is a number.')
except:
    print('Val is not a number')
    print('Enter a new number')
```

At the top of the statement is the code that we are interested in executing, which is run in the `try` statement. If that fails then the `except` statement comes into effect, (hopefully) returning helpful information to the user about what happened and giving them some guidance on how to avoid the problem in future.

The `except` statement will catch all errors and so we do not, initially at least, need to know exactly what errors we are trying to avoid. However, python does provide error codes, which we can use to expand the structure to capture specific error types. For the example above, we would want to capture a `TypeError`:

```python
try:
    if val>0 and val<10:
        print('Value: ', val, 'is a digit.')
    elif val==0:
        print('Value ', val, 'is nul')
    else:
        print('Value: ', val, 'is a number.')
except TypeError as err:
    print('Val is not a number')
    print('But our code does not crash anymore')
    print('The run-time error is:', err)
```

As with `if` statements, multiple `except` statements can be used, each with a different error test. These can be followed by an (optional) catch-all bare `except` statement (as we started with) to catch any unexpected errors. Note that only one `try` statement is allowed in the structure.

`try-except` structures may also contain `else` and `finally` statements (following in order after the `except` statements). The `else` statement will be executed if no `except` statements are executed, while the `finally` statement will be executed after all other statements are finished.

```python
try:
    if val>0 and val<10:
        print('Value: ', val, 'is a digit.')
    elif val==0:
        print('Value ', val, 'is nul')
    else:
        print('Value: ', val, 'is a number.')
except TypeError as err:
    print('Val is not a number')
    print('But our code does not crash anymore')
    print('The run-time error is:', err)
else:
    print('1/val = ', 1/val)
finally:
    print('release memory')
    del(val)
```

The typical use of the `finally` statement is to deal with the release of external resources (such as files or network connections) whether or not the attempted action has been successful.

More details about errors and exceptions can be found in the [Python tutorials](https://docs.python.org/3/tutorial/errors.html).

:::::::::::::::::::::::::::::::::::::::  challenge

## HTTP errors

Earlier we were introduced to the `requests` library. This is used to access web
content, and returns a code indicating the request status. The `raise_for_status`
function is provided for testing the request status, which returns a `request.HTTPError`
if there is an error. For example, if we request a missing webpage we get this error:

```python
import requests

source_url='https://api.datacite.org/dois/10.48546/workflowhub.workflow.56.1000'
response = requests.get(source_url)
response.raise_for_status()
```

```output
Traceback (most recent call last):

  File "<ipython-input-108-06c23faa067a>", line 3, in <module>
    response.raise_for_status()


  File "/Users/mbessdl2/anaconda3/envs/myenv/lib/python3.9/site-packages/requests/models.py", line 943, in raise_for_status
    raise HTTPError(http_error_msg, response=self)

HTTPError: 404 Client Error: Not Found for url: https://api.datacite.org/dois/10.48546/workflowhub.workflow.56.1000
```

Write a `try-except` structure that uses `raise_for_status` and deals with the error without crashing.

:::::::::::::::  solution

## Solution

```python
import requests

try:
    source_url='https://api.datacite.org/dois/10.48546/workflowhub.workflow.56.1000'
    response = requests.get(source_url)
    response.raise_for_status()
except requests.HTTPError as err:
    print(err)
```

```output
404 Client Error: Not Found for url: https://api.datacite.org/dois/10.48546/workflowhub.workflow.56.1000
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Assert

The `try-except` structure is useful for controlling small chunks of code, where we might reasonably expect that the user can fix the problem (where it is a program which takes a short time to run), or where our program itself will fix the problem (such as substituting NaN values for a failed calculation).

Sometimes, however, it is better to check the data before it is passed into the code and to trigger an early crash in the program, which returns a meaningful error. For example, this can be extremely useful in the case of large numerical calculations that take a long time to run and which will only use some of the provided variables after a significant amount of the computational work has already been carried out. In such situations your user will appreciate being warned early on that they have provided invalid data.

To conduct such a test we can use an `assert` statement. This follows the structure: `assert <test>, <error message>`. As an example of this, we can write the test for non-numerical values as:

```python
val = 'a'

assert type(val) is float or type(val) is int, "Variable has to be a numerical object"

if val>0 and val<10:
    print('Value: ', val, 'is a digit.')
elif val==0:
    print('Value ', val, 'is nul')
else:
    print('Value: ', val, 'is a number.')
```

```output
Traceback (most recent call last):

  File "<ipython-input-38-52d27526daab>", line 1, in <module>
    assert type(val) is float or type(val) is int, "Variable has to be a numerical object"

AssertionError: Variable has to be a numerical object
```

Like the earlier `if` statement, this catches the problematic variable before it reaches our calculation. However it is far less intrusive code, and assert tests can be added in sequence, so that we can quickly and easily increase the coverage of such tests.

:::::::::::::::::::::::::::::::::::::::  challenge

## Testing for NaN's (Not a Number)

Numpy provides Not a Number (nan) values, which can be used to indicate missing data.
These are not caught by the test above:

```python
import numpy as np

val = np.nan

assert type(val) is float or type(val) is int, "Variable has to be a numerical object"

if val>0 and val<10:
  print('Value: ', val, 'is a digit.')
elif val==0:
  print('Value ', val, 'is nul')
else:
  print('Value: ', val, 'is a number.')
```

```output
Value:  nan is a number.
```

However numpy does provide the `isnan()` function for testing if a value is NaN. Add an assertion test using this which will raise and error if the value is NaN.

:::::::::::::::  solution

## Solution

Because the test checks if a value is NaN, we need to add `not` before the test, to ensure
that the `assert` statement fails if it is a NaN:

```python
import numpy as np

val = np.nan

assert type(val) is float or type(val) is int, "Variable has to be a numerical object"
assert not np.isnan(val), "Variable must not be a NaN"

if val>0 and val<10:
    print('Value: ', val, 'is a digit.')
elif val==0:
    print('Value ', val, 'is nul')
else:
    print('Value: ', val, 'is a number.')
```

```output
Traceback (most recent call last):

  File "<ipython-input-52-553a9c93f77a>", line 4, in <module>
    assert not np.isnan(val), "Variable must not be a NaN"

AssertionError: Variable must not be a NaN
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- `try-except` structures are useful for catching errors as they occur
- `assert` structures are useful for forcing errors early, to avoid wasted effort

::::::::::::::::::::::::::::::::::::::::::::::::::


