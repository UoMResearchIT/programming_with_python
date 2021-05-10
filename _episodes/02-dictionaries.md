---
title: "Dictionaries"
teaching: 0
exercises: 0
questions:
- "How can I work with relational datasets?"
- "How can I access remote data directly in my scripts?"
objectives:
- "Learn about JSON format"
- "How to create dictionaries"
- "Loading JSON data and working with it"
- "Using web API's for accessing remote data"
keypoints:
- "JSON is simple"
- "Dictionaries are defined using `key`:`value` pairs"
- "Dictionaries can be nested, and mixed with lists"
- "Web API's can be accessed using the `requests` library"
---

In previous lessons we have learnt about lists and numpy multi-dimensional arrays, which are designed for working with structured, tabular, datasets. But much of the data that we use in our day to day lives, such as that in data catalogs or the communications between modern digital services, does not fit nicely into these tabular datasets. Instead unstructured data formats, which use labels or 'keys' to identify each data object are needed. One of the most common formats for such data is the [JavaScript Object Notation (JSON)](https://en.wikipedia.org/wiki/JSON) file format. This format was originally developed to fulfil the need for a self-contained, flexible format for real-time server-to-browser communication, and is now used as the basis for many unstructured data formats.

One example of such usage in research is the storage of metadata for data, programs, workflows, or any other such object in a [Research Object Crate (RO-Crate)](https://www.researchobject.org/ro-crate/). These metadata records take the form:
~~~
{
  "@context": "https://w3id.org/ro/crate/1.1/context",
  "@graph": [
    {
      "@id": "ro-crate-metadata.json",
      "@type": "CreativeWork",
      "about": {
        "@id": "./"
      },
      "conformsTo": {
        "@id": "https://w3id.org/ro/crate/1.1"
      }
    },
    {
      "@id": "./",
      "@type": "Dataset",
      "mainEntity": {
        "@id": "tracking_workflow.ga"
      },
      "hasPart": [
        {
          "@id": "tracking_workflow.ga"
        },
        {
          "@id": "object_tracking_pipeline.png"
        }
      ],
      "author": [

      ],
      "provider": [
        {
          "@id": "#project-1"
        }
      ],
      "license": "Apache-2.0",
      "sdPublisher": {
        "@id": "#person-1"
      },
      "sdDatePublished": "2021-01-01 00:00:00 +0000"
    },
    {
      "@id": "#galaxy",
      "@type": "ComputerLanguage",
      "name": "Galaxy",
      "identifier": {
        "@id": "https://galaxyproject.org/"
      },
      "url": {
        "@id": "https://galaxyproject.org/"
      }
    },
    {
      "@id": "#project-1",
      "@type": "Organization",
      "name": "Science Workflows",
    },
    {
      "@id": "#person-1",
      "@type": "Person",
      "name": "Alice Smith",
    }
  ]
}
~~~
{: .language-json}

The layout of this object is organised using `key`:`value` pairs, where the `key` is a unique string, and the `value` can be any data type, including other data structures. This simple layout allows quite complex data objects to be constructed.


## Dictionary

In python this structure is implemented using the 'dictionary' object. Below we will go through the principles of creating and working with these objects. Then we will introduce a library for working with JSON files.

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

Following the previous example, we can create a python dictionary using the name of a person as the *key* and their age as the *value*:
~~~
d = {'alice': 35, 'bob': 18}
~~~
{: .language-python}

~~~
print(d)
~~~
{: .language-python}

~~~
{'alice': 35, 'bob': 18}
~~~
{: .output}

Alternatively, a dictionary object can be created using the `dict` function, in a similar manner to using the `list` function. When using the `dict` function we need to indicate which *key* is associated with which *value*. This can be done in a number of ways, firstly with tuples:
~~~
d2 = dict([('alice', 35), ('jane', 24), ('bob',18)])
~~~
{: .language-python}
or with direct association:
~~~
d3 = dict(bob=18, alice=35, jane=24)
~~~
{: .language-python}
or using the special `zip` function, which can be used to create a set of tuples from the given iterable lists:
~~~
d4 = dict(zip(['jane','alice','bob'],[24,35,18]))
~~~
{: .language-python}

### Accessing elements

To access an element of the dictionary we must use the *key*:
~~~
key = 'alice'
print('The name of the person is used as key:', key)
print('The value associated to that key is:', d[key])
~~~
{: .language-python}
~~~
The name of the person is used as key: alice
The value associated to that key is: 35
~~~
{: .output}

### Adding an element

Adding an element to a dictionary is done by creating a new key and attaching a value to it.

~~~
print('Original dictionary:', d)
d['jane'] = 24
print('New dictionary:', d)
~~~
{: .language-python}
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
{: .language-python}
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
> {: .language-python}
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
> Keys have to be unique; you cannot have two keys with the same name. If you try to add an item using a key already present in the dictionary you will overwrite the previous value.
>
> ~~~
> print('Original dictionary:', d)
> d['alice'] = 12
> print('New dictionary:', d)
> ~~~
> {: .language-python}
> ~~~
> Original dictionary: {'alice': 35, 'bob': 18, 'jane': 24}
> New dictionary: {'alice': 12, 'bob': 18, 'jane': 24}
> ~~~
> {: .output}
{: .callout}

## Equality between dictionaries

To be equal, all the elements which compose the first dictionary must be present in the second, and only those elements.

The position (ordering) is not important.

~~~
d1 = {'alice': 12, 'bob': 18, 'jane': 24, 'tom': 54, 'david': 87}
d2 = {'tom': 54, 'david': 87}
d3 = {'bob': 18, 'alice': 35, 'jane': 24}
d4 = {'alice': 35, 'bob': 18, 'jane': 24}
print('Dictionary 1 and dictionary 2 are equal:', d1 == d2)
print('Dictionary 1 and dictionary 3 are equal:', d1 == d3)
print('Dictionary 3 and dictionary 4 are equal:', d3 == d4)
~~~
{: .language-python}
~~~
Dictionary 1 and dictionary 2 are equal: False
Dictionary 1 and dictionary 3 are equal: False
Dictionary 3 and dictionary 4 are equal: True
~~~
{: .output}

## Splitting out keys and values

Dictionaries have their own methods. Two of the most useful are keys and values which, as their name suggest, extract all the keys and all the values in an iterator.

~~~
d.keys()
~~~
{: .language-python}
~~~
dict_keys(['alice', 'bob', 'jane', 'tom', 'david'])
~~~
{: .output}

~~~
d.values()
~~~
{: .language-python}
~~~
dict_values([12, 18, 24, 54, 87])
~~~
{: .output}

## Presence (or not) of an element inside a dictionary

It is possible to test if a key is present in the dictionary (or not) using the keyword `in`, just as we did at the start of this lesson for values within a list:
~~~
'alice' in d
~~~
{: .language-python}
~~~
True
~~~
{: .output}

~~~
'mark' in d
~~~
{: .language-python}
~~~
False
~~~
{: .output}

Note, however, that we can't directly test for the presence of values:
~~~
12 in d
~~~
{: .language-python}
~~~
False
~~~
{: .output}
Instead we would have to use the `values` method to search these:
~~~
12 in d.values()
~~~
{: .language-python}
~~~
True
~~~
{: .output}


## JSON files

Because JSON files are such a widely used format, python has a built in package for working with JSON, called `json`:
~~~
import json
import glob

filenames = sorted(glob.glob('*.json'))

j_objects = {}
for filename in filenames:
    with open(filename) as f:
        j_objects[filename] = json.loads(f.read())
~~~
{: .language-python}

The `f.read` method reads the file as a single string, which the `json.loads()` method then turns into a dictionary, following the JSON standard.


## HTTP requests

Although lot of information is available on the internet for general use, without automated tools for accessing this data it is difficult to make full use of it. Python has a number of libraries for making HTTP requests, to help with this automation, of which the `requests` library is the most commonly used. This library provides a streamlined application process interface (API) for carrying out these tasks, and has built in JSON support, for easy digesting of the retrieved data.

The basic interaction for making a HTTP request is:
~~~
import requests

source_url='https://api.datacite.org/dois/10.48546/workflowhub.workflow.56.1'
requests.get(source_url)
~~~
{: .language-python}
~~~
<Response [200]>
~~~
{: .output}
The HTTP request returns a response code - a value of 200 indicates the request was successful. There are a wide range of [possible response codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes). Those starting as 2XX generally indicate success, whereas those starting with 4XX indicate a failure of some sort (including the most common: `404 Not Found`).

The HTTP request we made returned more than just the response code, there will also be the attached content that we requested. In this case our request was made to an API which returns citation information associated with the DOI `10.48546/workflowhub.workflow.56.1`.
Rather than being presented as a complex webpage, this information is returned as a machine-readable string, similar to the JSON file we read earlier, so we can read this in a similar manner:
~~~
response = requests.get(source_url)
record = response.json()
~~~
{: .language-python}

Once the data is in a dictionary we can start exploring it - first step is to check the keys available:
~~~
record.keys()
~~~
{: .language-python}
~~~
dict_keys(['data'])
~~~
{: .output}
The upper level of the dictionary is simply data - so we can move to the second level:
~~~
record['data'].keys()
~~~
{: .language-python}
~~~
dict_keys(['id', 'type', 'attributes', 'relationships'])
~~~
{: .output}

The `id` contains the DOI that we used to find this entry, while the `attributes` contains the metadata for the object referred to by the DOI. By digging further into the dictionary we can extract information about the object.

> ## Find the title
> What is the path to find the title of the object?
> > ## solution
> > ~~~
> > record['data']['attributes']['titles'][0]['title']
> > ~~~
> > {: .language-python}
> >
> {: .solution}
{: .challenge}

> ## Find the title (part 2)
> Assuming that all JSON objects returned by this API follow the same layout as this
> record, write a simple function that will return the title of any DOI it is given.
>
> Test that your function works using the DOI: `10.5281/zenodo.4416028`
> > ## solution
> > ~~~
> > def doi_title( doi_string ):
> >     source_url = 'https://api.datacite.org/dois/{}'.format(doi_string)
> >     response = requests.get(source_url)
> >     record = response.json()
> >     return(record['data']['attributes']['titles'][0]['title'])
> > ~~~
> > {: .language-python}
> > Testing the function:
> > ~~~
> > doi_title('10.5281/zenodo.4416028')
> > ~~~
> > {: .language-python}
> > ~~~
> > 'Britain Breathing 2016-2019 Air Quality and Meteorological Dataset'
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}





# Speed Tests...

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

This is a real problem because the membership test is a very useful and common procedure. So we would like to have something which is not dependent on the number of elements.


> ## Testing access time for large dictionaries
> Create two dictionaries, one with 100,000 key:value pairs, the other with
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

