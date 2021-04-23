---
title: "Units and Quantities"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

One of the most common problems in physics and the development of physical models is related to the conversion and management of physical units. Without clear management of physical units, and ensuring that the information passed between different parts of the system is consistent, accidents such as the [Mars Climate Orbiter probe crash](https://en.wikipedia.org/wiki/Mars_Climate_Orbiter) can occur.

In this lesson we will look at one of the available libraries in python for helping the avoidance of such problems. This is the [units](https://docs.astropy.org/en/stable/units/index.html) module in the [astropy](https://docs.astropy.org/en/stable/index.html) library.

## Installing a new python library

Following the conda lesson, we install the astropy library using:
~~~
conda install astropy
~~~
{: .language-bash}

The `units` module can then be imported in your scripts using:
~~~
from astropy import units as u
~~~
{: .language-python}
Importing as `u` is not necessary, but is a common shorthand for the module, and so we'll use it below.

## Basics

Units can be added to a python variable by multiplying that variable by the unit:
~~~
length = 26.2 * u.meter
~~~
{: .language-python}
(note the use of US spellings for units).

When printing the variable the unit information will now be attached:
~~~
print(length)
~~~
{: .language-python}

The type of this new variable is an `astropy` `Quantity`:
~~~
type(length)
~~~
{: .language-python}
~~~
astropy.units.quantity.Quantity
~~~
{: .output}

You can still access the original object using `value`:
~~~
print(length.value)
type(length.value)
~~~
{: .language-python}
~~~
26.2
float
~~~
{: .output}

Quantities can be converted to other units, systems or factors, by using the function `to()`. For example:
~~~
length.to(u.km)
length.to(u.lightyear)
~~~
{: .language-python}

This enables the use of arithmetic operations when quantities have compatible units:
~~~
distance_start = 10 * u.mm
distance_end = 23 * u.km
length = distance_end - distance_start
print(length)
~~~
{: .language-python}

And it also enables the combining of quantities, for example, to calculate a speed:
~~~
time = 15 * u.minute
speed = length / time
print(speed)
~~~
{: .language-python}

By default the units library will select units to report for these values based on what the units are of the objects that you have passed it. You can, as before, convert these to the units of your choice:
~~~
print(speed.to(u.km/u.s))
~~~
{: .language-python}

You can also convert the units to the base (irreducible) units for the unit system you are using with the `decompose` function (changing the unit system choice will be covered later):
~~~
print(speed.decompose())
~~~
{: .language-python}

> ## Choosing different base units
> If you wish to reduce to different base units you can pass a list of those units to the `bases` parameter when calling the `decompose` function:
> ~~~
> print(speed.decompose(bases=['km','s']))
> ~~~
> {: .language-python}
> Note that the order of the values in the list passed to `bases` parameter doesn't matter. However, the base units you chose must be either one of the original units used, or a standard base units. This function cannot be used to convert from km to cm, for example. Instead it is useful where you only want to reduce select units within the object.
{: .callout}

You can change the base system using functions such as `si` (to change to the default SI units) and `cgs` (to change to centimetre-grams-seconds units):
~~~
print(speed.cgs)
~~~
{: .language-python}


> ## Reducing more complex quantities
> We have been given some (slightly messy) code that calculates the
> [jerk](https://en.m.wikipedia.org/wiki/Jerk_(physics)), or rate of change of
> acceleration, of a vehicle in the units `km/s/min^2`:
> ~~~
> acceleration = (25 * u.meter).to(u.km) / (15 * u.minute)**2
> time_change = 1 * u.hour
> jerk = acceleration / time_change
> print(jerk)
> ~~~
> {: .language-python}
> ~~~
> 0.00011111111111111112 km / (min2 s)
> ~~~
> {: .output}
> For our work we need to convert this to the units `km/hour^3`
>
>
>
{: .challenge}

## Imperial Units

The `units` library is focused on scientific notation. To use units from the (US) imperial system we can import the `imperial` library:
~~~
from astropy.units import imperial
~~~
{: .language-python}
These can then be used in the same manner as the standard units:
~~~
speed.to(imperial.mile/u.hour)
~~~
{: .language-python}


## Equivalent Units

There are some unit conversions that would initially appear to be unconvertible. For example, it is possible to convert meters into Hertz. At first glance it seems to be wrong but if you know the quantities for wavelength and frequencies, it is indeed a valid conversion:
[\lambda = \frac{c}{\nu}]
where:
- $lambda \sim m$
- $c \sim \frac{m}{s}$
- $\nu \sim Hz \sim s^{-1}$

This, however, does not work as above:
~~~
(656.281 * u.nm).to(u.Hz)
~~~
{: .language-python}

Instead we need to inform `units` what unit equivalency we want it to use, which can be specified with the `equivalencies` option:
~~~
(656.281 * u.nm).to(u.Hz, equivalencies=u.spectral())
~~~
{: .language-python}
In this case we use the `spectral` equivalence library, which allows conversions between wavelengths, wave number, frequency, and energy equivalent pairs. There are a number of other built-in equivalency libraries, for commonly used pairings.



{% include links.md %}

