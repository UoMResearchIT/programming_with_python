---
title: "Defensive Programming"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

In this episode we are going to learn a python structure which will help us to properly manage the errors that can be encountered when executing our code.

In science most developers write code for themselves and do not feel the need to use such tools. Nowadays scientists have to share their work more and funding agencies are starting to ask for the code used in published experiments.

Furthermore, applications are not expected to crash and we are going to learn how to avoid this non desirable behaviour.

> ## What is the problem with this test?
>
> Please look at the following code. Can you find the fundamental problem in this test?
>
> ~~~
> val = 1
>
> if val>0 and val<10:
>     print('Value: ', val, 'is a digit.')
> elif val==0:
>     print('Value ', val, 'is nul')
> else:
>     print('Value: ', val, 'is a number.')
> ~~~
> {: .language-bash}
>
> > ## Solution
> > The test assumes that `val` is a number, and throws an uncontrolled error if it is not.
> > ~~~
> > val = 'a'
> >
> > if val>0 and val<10:
> >     print('Value: ', val, 'is a digit.')
> > elif val==0:
> >     print('Value ', val, 'is nul')
> > else:
> >     print('Value: ', val, 'is a number.')
> > ~~~
> > {: .language-bash}
> > ~~~
> > ---------------------------------------------------------------------------
> > TypeError                                 Traceback (most recent call last)
> > <ipython-input-2-99c0e25bf5e9> in <module>()
> >       1 val = 'a'
> >       2
> > ----> 3 if val>0 and val<10:
> >       4     print('Value: ', val, 'is a digit.')
> >       5 elif val==0:
> >
> > TypeError: '>' not supported between instances of 'str' and 'int'
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

This problem can be avoided using an `if` statement:
~~~
if type(val) is int or type(val) is float:
    ...
else:
    print('val is not a number')
~~~
{: .language-python}
However, this becomes tedious to implement very quickly, especially when trying to cover a number of similar possible errors.

Python provides a structure which allows a developer to properly manage any possible errors. The basic `try-except` structure is:
~~~
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
~~~
{: .language-python}

This can be expanded to capture specific error types. For the example above, we would want to capture a `TypeError`, e.g.:
~~~
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
~~~
{: .language-python}

As with `if` statements, multiple `except` statements can be used, each with a different error test, with an (optional) catch all `except` to catch any unexpected errors. Note that only one `try` statement is allowed.

`try` statements may also contain `else` and `finally` statements (following in order after the `except` statements). The `else` statement will be executed if no `except` statements are executed, while the `finally` statement will be executed after all other statements are finished.

~~~
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
    print('1/val = ', val)
finally:
    print('release memory')
    del(val)
~~~
{: .language-python}

The typical use of the `finally` statement is the release of external resources (such as files or network connections) whether or not the attempted action has been successful.

More details about errors and exceptions can be found in the [Python tutorials](https://docs.python.org/3/tutorial/errors.html).


## Assert

But sometimes, the developer prefers to have a crash with a meaningful error. It is possible to do a test before using the variable.

(For example, this can be extremely useful in the case of large numerical calculations that take a long time to run and which will use certain types of variables in middle of them.)

~~~
val = np.nan
print(type(val))



assert type(val) is float or type(val) is int, "Variable has to be a numerical object" # or val is int

assert not np.isnan(val), "Variable must not be a NaN"

if val>0 and val<10:
    print('Value: ', val, 'is a digit.')
elif val==0:
    print('Value ', val, 'is nul')
else:
    print('Value: ', val, 'is a number.')
~~~
{: .langauge-python}



## Tests

Make use of the [built-in exceptions documentation](https://docs.python.org/3.3/library/exceptions.html#built-in-exceptions)?

Add a multiple choice question - give some code and ask which is the best exception to search for?





{% include links.md %}

