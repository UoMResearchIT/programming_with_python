---
title: "Units and Quantities"
teaching: 0
exercises: 0
questions:
- "How can I ensure consistent units in my code?"
objectives:
- "Creating quantity objects which include values and units"
- "Applying unit conversions"
- "Learn about different types of equivalence for units"
- "Using units for annotating plots"
- "Test different unit libraries"
keypoints:
- "`astropy.units` library provides unit support"
- "`Quantity` objects are created by multiplying values by the desired units"
- "The `.to()` function can be used to convert units"
- "The `.decompose()` function can be used to convert to the base (irreducible) units"
- "Equivalences can be found using the `.find_equivalent_units()` function"
- "Specific equivalence libraries can be defined using the `equivalences=` keyword"
- "Import quantity-support from `astropy.visualization` to integrate units with matplotlib for data plotting"
- "The `pint` library provides similar unit support, but is better for working with temperature increments"
---

One of the most common problems in physics and the development of physical models is related to the conversion and management of physical units. Without clear management of physical units, and ensuring that the information passed between different parts of the system is consistent, accidents such as the [Mars Climate Orbiter probe crash](https://en.wikipedia.org/wiki/Mars_Climate_Orbiter) can occur.

In this lesson we will look at one of the available libraries in python for helping to avoid such problems. This is the [units](https://docs.astropy.org/en/stable/units/index.html) module in the [astropy](https://docs.astropy.org/en/stable/index.html) library.

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
Importing as `u` is not necessary, but it is a common shorthand for this module, and so we'll use it below.

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
> Note that the order of the values in the list passed to `bases` parameter doesn't matter. However, the base units you choose must be either one of the original units used, or a standard base unit. This function cannot be used to convert from km to cm, for example. Instead it is useful where you only want to reduce select units within the object.
{: .callout}

You can change the base system using functions such as `si` (to change to the default SI units) and `cgs` (to change to centimetre-grams-seconds units):
~~~
print(speed.cgs)
~~~
{: .language-python}


> ## Reducing more complex quantities
> We have been given some (slightly messy) code that calculates the
> [jerk](https://en.m.wikipedia.org/wiki/Jerk_(physics)), or rate of change of
> acceleration, of a vehicle in the units `m/h/min^2`:
> ~~~
> acceleration = (25 * u.meter) / (15 * u.minute) / (15 * u.minute)
> time_change = 1 * u.hour
> jerk = acceleration / time_change
> print(jerk)
> ~~~
> {: .language-python}
> ~~~
> 0.1111111111111111 m / (h min2)
> ~~~
> {: .output}
> For the report we are writing we need to convert this to the units `km/hour^3`, which of
> these changes will do this task for us?
> 1. `jerk.decompose(bases=['km','hr'])`
> 2. `jerk.to(u.km/u.hour**3)`
> 3. `jerk.to(u.km/u.hour**2)`
>
> > ## Solution
> > Answer 2 is correct. Decompose will only reduce to base units (or existing units), and
> > so can't change minutes to hours, or metres to kilometres. Answer 3 won't work,
> > because we have to apply the modification to all units (including those which are
> > already correct).
> {: .solution}
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
![Equation for speed of light.](../fig/light_speed_eqn.png)
where:
- *lambda* ~ m
- c ~ m/s
-  ~ *Hz* ~ 1/s

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

Other built-in equivalencies are:
- Doppler (`dopplr_radio`, `doppler_optical`, `doppler_relativistic`)
- spectral flux density
- temperature
- brightness temperature
- temperature energy
- and you can [build your own](https://docs.astropy.org/en/stable/units/equivalencies.html#writing-new-equivalencies)

Finding equivalencies is possible using the `find_equivalent_units` function:
~~~
u.Hz.find_equivalent_units()
~~~
{: .language-python}
~~~
  Primary name | Unit definition | Aliases
[
  Bq           | 1 / s           | becquerel    ,
  Ci           | 3.7e+10 / s     | curie        ,
  Hz           | 1 / s           | Hertz, hertz ,
]
~~~
{: .output}

This is a quite limited list - to see what is available for other equivalence libraries, we must specify these when making the request:

~~~
u.Hz.find_equivalent_units(equivalencies=u.spectral())
~~~
{: .language-python}
~~~
  Primary name | Unit definition        | Aliases
[
  AU           | 1.49598e+11 m          | au, astronomical_unit            ,
  Angstrom     | 1e-10 m                | AA, angstrom                     ,
  Bq           | 1 / s                  | becquerel                        ,
  Ci           | 3.7e+10 / s            | curie                            ,
  Hz           | 1 / s                  | Hertz, hertz                     ,
  J            | kg m2 / s2             | Joule, joule                     ,
  Ry           | 2.17987e-18 kg m2 / s2 | rydberg                          ,
  cm           | 0.01 m                 | centimeter                       ,
  eV           | 1.60218e-19 kg m2 / s2 | electronvolt                     ,
  earthRad     | 6.3781e+06 m           | R_earth, Rearth                  ,
  erg          | 1e-07 kg m2 / s2       |                                  ,
  jupiterRad   | 7.1492e+07 m           | R_jup, Rjup, R_jupiter, Rjupiter ,
  k            | 100 / m                | Kayser, kayser                   ,
  lyr          | 9.46073e+15 m          | lightyear                        ,
  m            | irreducible            | meter                            ,
  micron       | 1e-06 m                |                                  ,
  pc           | 3.08568e+16 m          | parsec                           ,
  solRad       | 6.957e+08 m            | R_sun, Rsun                      ,
]
~~~
{: .output}

## Angles

The numpy trigonometry functions (`np.sin`, `np.cos`, etc) require angles that are passed to them to be in radians, rather than degrees. It is inconvenient to have to explicitly convert from other units to radians every time that you wish to use these functions, but with units you can have this done implicitly for you.

The radian unit is `u.rad`, and it's equivalencies are:
~~~
u.rad.find_equivalent_units()
~~~
{: .language-python}
~~~
  Primary name | Unit definition | Aliases
[
  arcmin       | 0.000290888 rad | arcminute ,
  arcsec       | 4.84814e-06 rad | arcsecond ,
  cycle        | 6.28319 rad     | cy        ,
  deg          | 0.0174533 rad   | degree    ,
  hourangle    | 0.261799 rad    |           ,
  mas          | 4.84814e-09 rad |           ,
  rad          | irreducible     | radian    ,
  uas          | 4.84814e-12 rad |           ,
]
~~~
{: .output}
We can see from this that the degree unit is `u.deg`, so we can use this to define our angles:
~~~
angle = 90 * u.deg()
print('angle in degrees: {}; and in radians: {}'.format(angle.value,angle.to(u.rad).value))
~~~
{: .language-python}
~~~
angle in degrees: 90.0; and in radians: 1.5707963267948966
~~~
{: .output}

Now we can pass the angle directly to `np.sin` without having to convert directly to radians:
~~~
print('sin of 90 degrees is: {}'.format(np.sin(angle)))
print('sin of pi/2 radians is: {}'.format(np.sin(1.57079632)))
print('sin of 90 degrees is not: {}'.format(np.sin(90)))
~~~
{: .language-python}
~~~
sin of 90 degrees is: 1.0
sin of pi/2 radians is: 1.0
sin of 90 degrees is not: 0.8939966636005579
~~~
{: .output}


### Plotting Quantities

Support has been added for using the units information in matplotlib plots. To use this we need to import the `quantity_support` library:
~~~
from astropy.visualization import quantity_support
~~~
{: .language-python}

We will create a numpy array of the angles between 0-180 degrees, and plot the sin of these:
~~~
angles = np.arange(181) * u.deg
~~~
{: .language-python}

~~~
plt.plot(angles, np.sin(angles))
~~~
{: .language-python}
![Plot of sin curve for degrees between 0-180](../fig/sin_curve_deg.png)
Note that the units for the x-axis are properly presented. This can be done for any angular unit we wish:
~~~
plt.plot(angles.to(u.rad), np.sin(angles))
~~~
{: .language-python}
![Plot of sin curve for degrees between 0-180](../fig/sin_curve_rad.png)


## Temperature

Each of the temperature scales is considered as using an irreducible unit in standard usage. As a consequence of this we need to always specify that we are using the `u.temperature` equivalences when we convert between these:
~~~
t2 = 1 * u.deg_C
print('{} is equivalent to {}'.format(t2,t2.to(imperial.Fahrenheit, equivalencies=u.temperature())))
~~~
{: .language-python}
~~~
1.0 deg_C is equivalent to 33.8 deg_F
~~~
{: .output}
But, be aware that these are scales, if we wanted to increase a temperature given in Fahrenheit by 1 degree Celsius, we would not add 33.8 degrees Fahrenheit!

To get the incremental value of 1 degree Celsius in Fahrenheit we would need to do this:
~~~
t1 = 0 * u.deg_C
t2 = 1 * u.deg_C
print('{} increment is equivalent to a {} increment'.format(
    t2 - t1,
    t2.to(imperial.Fahrenheit,equivalencies=u.temperature())
        - t1.to(imperial.Fahrenheit, equivalencies=u.temperature())
    )
)
~~~
{: .language-python}
~~~
1.0 deg_C increment is equivalent to a 1.7999999999999972 deg_F increment
~~~
{: .output}

This is verging on unreadable (as is demonstrated by us having to split a simple arithmetic expression involving two variables across two lines of code).

Fortunately there is support for cleaner temperature conversions in the [pint](https://pint.readthedocs.io/en/latest/index.html) package. This has similar functionality to the `astropy.unit` package, but has been built from the ground up with a focus on different aspects. These include unit parsing and standalone unit definitions, uncertainties integration, and (important for our example here), cleaner treatment of temperature units.

Pint can be installed using conda (and the conda-forge channel):
~~~
conda install -c conda-forge pint
~~~
{: .language-bash}

The unit registry is loaded using:
~~~
from pint import UnitRegistry
ureg = UnitRegistry()
~~~
{: .language-python}
A units definition file could be given here if needed, but in this case we will stick with the default settings.

This library has the temperature units as before:
~~~
t2 = 1 * ureg.degC
print('{} is equivalent to {}'.format(t2,t2.to(ureg.degF)))
~~~
{: .language-python}
~~~
1 degree_Celsius is equivalent to 33.79999999999993 degree_Fahrenheit
~~~
{: .output}

But it also includes the concept of temperature increments:
~~~
deltaT = 1 * ureg.delta_degC
print('{} is equivalent to {}'.format(deltaT,deltaT.to(ureg.delta_degF)))
~~~
{: .language-python}
~~~
1 delta_degree_Celsius is equivalent to 1.7999999999999998 delta_degree_Fahrenheit
~~~
{: .output}

Because of the popularity of python this overlap of functionality of packages can be common. We would recommend exploring the different available packages that might cover your requirements before settling on one. And keep watching out for new packages, and be prepared to switch when starting new projects if/when you find better packages.

{% include links.md %}

