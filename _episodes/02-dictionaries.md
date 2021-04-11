---
title: "Dictionaries"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

Previously we learned about sequence objects such as: string, list, tuple. Now we are going to learn about a new object which implements the notion of hash table.

Why do we need this new object?

Sequences are a great tool but they have one big limitation. The execution time to find one specific value inside is linear, as can be shown by fruitless searches for the string `x` within increasingly long lists of integers.
~~~
short_list = list(range(100_000))
long_list = list(range(1_000_000))
~~~
{: .language-python}

We use the built-in `%timeit` function, to test the speed of these searches:
~~~
%timeit -n100 'x' in short_list
~~~
{: .language-python}
~~~
2.02 ms ± 529 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
~~~
{: .output}

~~~
%timeit -n100 'x' in long_list
~~~
{: .language-python}
~~~
17.4 ms ± 1.35 ms per loop (mean ± std. dev. of 7 runs, 100 loops each)
~~~
{: .output}

Note that the increase in search time is (very roughly) linear.

This is a real problem because the membership test is a very useful and common procedure. So we would like to have something which is not dependent of the number of elements.

### Another limitation

We would like, for example to associate a key to an element like here the name alice with the age 35. We cannot do that with a list or any other sequence object.

We will get an error if we try:
~~~
t = []
t['alice'] = 35
~~~
{: .language-python}

~~~
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-28-bea9bcd2acd4> in <module>
      1 t = []
----> 2 t['alice'] = 35

TypeError: list indices must be integers or slices, not str
~~~
{: .output}

The hash table structure is the answer to these two limitations and in python it is implemented by the 'dictionary' object.

## Dictionary

### Creation

Lists are created by using square brackets `[ ]`.

Dictionaries are created by using curly brackets `{ }`, e.g.:
~~~
d = {}
~~~
{: .language-python}

The simplest way to create a dictionary with some value is:
~~~
d = {'keyname': 'keyvalue'}
~~~
{: .language-python}

Following the previous example, we can create a hash table, a python dictionary, using the name of the person as *key* and the age as *value*:
~~~
d = {'alice': 35, 'bob': 18}
~~~
{: .language-python}

~~~
print(d)
~~~
{: .language-bash}

~~~
{'alice': 35, 'bob': 18}
~~~
{: .output}

Alternatively, a dictionary object can be created using the `dict` function, in a similar manner to using the `list` function. In this case, however, we need to indicate which *key* is associated with which *value*. This can be done with tuples:
~~~
d2 = dict([('alice', 35), ('jane', 24), ('bob',18)])
~~~
{: .language-bash}
or with direct association:
~~~
d3 = dict(bob=18, alice=35, jane=24)
~~~
{: .language-bash}
or using the special `zip` method, which can be used to create a set of tuples from the given iterable lists:
~~~
d4 = dict(zip(['jane','alice','bob'],[24,35,18]))
~~~
{: .language-bash}

### Accessing elements

To access an element of the dictionary we must use the *key*:
~~~
key = 'alice'
print('The name of the person is used as key:', key)
print('The value associated to that key is:', d[key])
~~~
{: .language-bash}
~~~
The name of the person is used as key: alice
The value associated to that key is: 35
~~~
{: .output}

### Adding an element

Adding an element to a dictionary is done by creating a new key and affecting a value to it.

~~~
print('Original dictionary:', d)
d['jane']=24
print('New dictionary:', d)
~~~
{: .language-bash}
~~~
Original dictionary: {'alice': 35, 'bob': 18}
New dictionary: {'alice': 35, 'bob': 18, 'jane': 24}
~~~
{: .output}

To add one or more new elements we can also use the `update` method:
~~~
d_extra = {'tom': 54, 'david': 87}

d.update(d_extra)
print('Updated dictionary:', d)
~~~
{: .language-bash}
~~~
Updated dictionary: {'alice': 35, 'bob': 18, 'jane': 24, 'tom': 54, 'david': 87}
~~~
{: .output}

> ## Dictionary Concatenate Warning
> Unlike lists it is not possible to use the `+` operator to concatenate dictionaries:
>
> ~~~
> {'alice': 35} + {'bob': 18}
> ~~~
> {: .language-bash}
>
> ~~~
> ---------------------------------------------------------------------------
> TypeError                                 Traceback (most recent call last)
> <ipython-input-39-a6305e6df312> in <module>
> ----> 1 {'alice': 35} + {'bob': 18}
>
> TypeError: unsupported operand type(s) for +: 'dict' and 'dict'
> ~~~
> {: .output}
{: .callout}

> ## Key Uniqueness Warning
> Key have to be unique; you cannot have two keys with the same name. If you try to add a > key with a name already used you will overwrite the value of the previous one.
>
> ~~~
> print('Original dictionary:', d)
> d['alice']=12
> print('New dictionary:', d)
> ~~~
> {: .language-bash}
> ~~~
> Original dictionary: {'alice': 35, 'bob': 18, 'jane': 24}
> New dictionary: {'alice': 12, 'bob': 18, 'jane': 24}
> ~~~
> {: .output}
{: .callout}

## Equality between dictionaries

To be equal, all the elements which compose the first dictionay must be present in the second, and only those elements.

The position (ordering) is not important.

~~~
d1 = {'alice': 12, 'bob': 18, 'jane': 24, 'tom': 54, 'david': 87}
d2 = {'tom': 54, 'david': 87}
d3 = {'bob': 18, 'alice': 35, 'jane': 24}
d4 = {'alice': 35, 'bob': 18, 'jane': 24}
print('Dictionary 1 and dictionary 2 are not equal:', d1 == d2)
print('Dictionary 1 and dictionary 3 are not equal:', d1 == d3)
print('Dictionary 3 and dictionary 4 are equal:', d3 == d4)
~~~
{: .language-bash}
~~~
Dictionary 1 and dictionary 2 are not equal: False
Dictionary 1 and dictionary 3 are not equal: False
Dictionary 3 and dictionary 4 are equal: True
~~~
{: .output}

## Splitting out keys and values

Dictionaries have their own methods. Two of the most useful are keys and values which, as their name suggest, extract all the keys and all the values in an iterator.

~~~
d.keys()
~~~
{: .language-bash}
~~~
dict_keys(['alice', 'bob', 'jane', 'tom', 'david'])
~~~
{: .output}

~~~
d.values()
~~~
{: .language-bash}
~~~
dict_values([12, 18, 24, 54, 87])
~~~
{: .output}

## Presence (or not) of an element inside a dictionary

It is possible to test if a key is present in the dictionary (or not) using the keyword `in`, just as we did at the start of this lesson for values within a list:
~~~
'alice' in d
~~~
{: .language-bash}
~~~
True
~~~
{: .output}

~~~
'mark' in d
~~~
{: .language-bash}
~~~
False
~~~
{: .output}

Note, however, that we can't directly test for the presence of values:
~~~
12 in d
~~~
{: .language-bash}
~~~
False
~~~
{: .output}
Instead we would have to use the `values` method to search these:
~~~
12 in d.values()
~~~
{: .language-bash}
~~~
True
~~~
{: .output}

> ## Testing access time for large dictionaries
> Please create two dictionaries, one with 100,000 key:value pairs, the other with
> 1,000,000 key:value pairs, using the lists created at the start of this lesson. Then
> use these to test the access times for dictionaries using the `%timeit` function.
> How do the access times compare with those for the lists, are they quicker or slower,
> and do the access times scale linearly with the size of the dictionary?
>
> > ## Solution
> > The dictionaries can be created using the `zip` method:
> > ~~~
> > short_hash = dict(zip(short_list,short_list))
> > long_hash = dict(zip(long_list,long_list))
> > ~~~
> > {: .language-python}
> >
> > The access times for the dictionaries are 100-1,000 times faster than for lists,
> > and the search time does not increase with dictionary size.
> > ~~~
> > %timeit -n100 'x' in short_hash
> > ~~~
> > {: .language-bash}
> > ~~~
> > 34.2 ns ± 2.31 ns per loop (mean ± std. dev. of 7 runs, 100 loops each)
> > ~~~
> > {: .output}
> >
> > ~~~
> > %timeit -n100 'x' in long_hash
> > ~~~
> > {: .language-bash}
> > ~~~
> > 48.4 ns ± 5.39 ns per loop (mean ± std. dev. of 7 runs, 100 loops each)
> > ~~~
> > {: .output}
> >
> > **Note:** it is likely that your first report back from the test will look like this:
> > ~~~
> > The slowest run took 308.65 times longer than the fastest. This could mean that an intermediate result is being cached.
> > 2.52 µs ± 5.91 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
> > ~~~
> > {: .output}
> >
> > This is because the secret behind the speed of searching a dictionary is the caching
> > of the keys after the first access to them. Running the tests a second time will give
> > above test results.
> >
> > This caching behaviour is very useful for datasets for are accessed regularly.
> {: .solution}
{: .challenge}


{% include links.md %}

