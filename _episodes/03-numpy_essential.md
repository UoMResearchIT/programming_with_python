---
title: "Numpy and Matplotlib Essential"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

As suggested in one of the previous challenges, the numpy library provides an object called array, which is very similar object to the list. The main difference is the set of operations that can be performed on them. Numpy arrays are oriented towards computation.

In this lesson we are going to learn a little more about how to use this fundamental library to do any numerical analysis in Python.

Numpy extends the Python language by providing new types (array, matrix, masked_array...), functions and methods to realise efficient numerical calculation using Python.

As we saw in the Introduction course, the most basic numpy type is called an array. The most basic array is a multi-dimensional object which contains numerical data.

[Numpy](https://www.numpy.org) is _the_ numerical library for Python. It is too big to be covered in one day, so today we will learn a few of the basic objects and functions.

Numpy is a foundation block for all other python libraries used in science and data science, e.g.:
- [scipy](https://www.scipy.org) Fundamental library for scientific computing (interation, optimisation...)
- [pandas](https://pandas.pydata.org) data structure and data analysis tools
- [matplotlib](https://matplotlib.org) Python 2D plotting

And more specialised libraries such as:
- [astropy](https://www.astropy.org) for astronomy
- [h5py](https://www.h5py.org) to interact with HDF5 format datafiles
- [scikit-learn](https://scikit-learn.org) for Machine Learning
- [TensorFlow](https://www.tensorflow.org) for Deep Learning

We are going to learn some of the basic commands not seen in the first course. We will introduce some of these libraries but keep in mind that we are just covering the basics so you can understand how to start using this library. If you find that your interest is piqued and/or they are applicable to your problem, you can use the documentation to learn more.

To start, we are going to import the two libraries `numpy` and `matplotlib` that will be used in this episode.

~~~
import numpy as np
import matplotlib.pylab as plt
~~~
{: .language-python}
Here we import the libraries using a common shorthand for them, `np` for `numpy`, and `plt` for `matplotlib.pylab`. You will encounter this in many python scripts using these libraries, and we will continue to use these shorthands below.

Numpy has a number of functions which behave similar to basic python functions. For example, to create a `list` of odd numbers from 1 to 9 we would use `range` and `list`:
~~~
odds = list(range(1,10,2))
print('odds is type {}, and contains {}'.format(type(odds),odds))
~~~
{: .language-python}
~~~
odds is type <class 'list'>, and contains [1, 3, 5, 7, 9]
~~~
{: .output}
To create a numpy array we can use `np.arange`:
~~~
odds_np = np.arange(1,10,2)
print('odds_np is type {}, and contains {}'.format(type(odds_np),odds_np))
~~~
{: .language-python}
~~~
odds_np is type <class 'numpy.ndarray'>, and contains [1 3 5 7 9]
~~~
Note that we did not need to convert the output of `np.arange`, as we did for the output of `range`, because `np.arange` returns directly a numpy array, rather than returning an iterator.

> ## Numpy Array Addition
>
> What is the difference between addition of numpy arrays and lists? What is the result
> of these two additions?
> ~~~
> odds + odds
> odds_np + odds_np
> ~~~
> {: .language-python}
> 1. `[1,3,5,7,9,1,3,5,7,9]` for both
> 2. `[1,3,5,7,9,1,3,5,7,9]` for odds, and `[2,6,10,14,18]` for odds_np
> 3. `[2,6,10,14,18]` for odds, and `[1,3,5,7,9,1,3,5,7,9]` for odds_np
> 4. `[2,6,10,14,18]` for both
>
> > ## Solution
> > The correct answer is (2)
> {: .solution}
{: .challenge}

### Reshaping Numpy Arrays

Numpy arrays are multidimensional objects for storing data (not necessarily numerical). The shape of the array can be modified using the method `reshape`:
~~~
arr = np.arange(1,18,2)
arr_2d = arr.reshape((3,3))
print('arr has shape {}, and contains:'.format(arr.shape))
print(arr)
print('arr_2d has shape {}, and contains:'.format(arr_2d.shape))
print(arr_2d)
~~~
{: .language-python}
~~~
arr has shape (9,), and contains:
[ 1  3  5  7  9 11 13 15 17]
arr_2d has shape (3, 3), and contains:
[[ 1  3  5]
 [ 7  9 11]
 [13 15 17]]
~~~
{: .output}

Checking and changing the type of the data within the array is relatively straightforward:
~~~
print(arr.dtype)
arr = arr.astype(float)
print(arr.dtype)
~~~
{: .language-bash}
~~~
int64
float64
~~~
{: .output}

### Working with Matricies

Numpy arrays are not matrix objects, but numpy does provide a matrix object, which has the characteristics of a mathematical matrix:
~~~
mat = np.matrix(arr_2d)
type(mat)
~~~
{: .language-python}
~~~
numpy.matrix
~~~
{: .output}

These can be used to carry out matrix multiplication:
~~~
mat * mat
~~~
{: .language-python}
~~~
matrix([[ 87., 105., 123.],
        [213., 267., 321.],
        [339., 429., 519.]])
~~~
{: .output}

## Masked Arrays

A very useful tool that numpy provides for working with experimental data is the masked array. When you are taking data from an experiment you always have some data which are not present, or with a bad signal to noise ratio, or that you cannot use for some other reason. This function allows you to select which data you wish to work with within your dataset.

Masked arrays associate a numpy array with another array composed only of boolean values (True or False). These tell numpy whether to use (or not) the respective element.

To demonstrate this we are going to create a Gaussian function and use it to generate an example dataset and generate a plot. We will then add some noise to it and use a masked array to filter out the noisy data.

Reminder: the Gaussian function is define by:
$$
g(x) = \frac{1}{\sqrt{2\pi\sigma^{2}}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}
$$

> ## Gaussian Function
> 1. Create a function called `gauss` which will take three arguments (inputs): *x*, *mu*, and *sigma*, as defined above. (x is an array, mu is the position of the centre of the curve/peak and sigma is the width of the bell)
> 2. Create a numpy array using the 'numpy' function 'linspace' which will contain 1000 points equally spaced between x=-100 and x=100. Hint: You can print the help documentation of a function with 'help(name_of_the_function)'
> 3. Using the above gauss function and the array, create a list which contains the value of the gauss from x=-100 to x=100.
> 4. Use the 'matplotlib' library to plot the curve with mu=0 and sigma=10.
{: .challenge}

challenge 1:
~~~
def gauss(x, mu=0, sigma=1):
    return (1./(sigma * np.sqrt(2 * np.pi)) *
            np.exp( - (x - mu)**2 / (2 * sigma**2)))
~~~
{: .language-python}

challenge 2:
~~~
x = np.linspace(-100, 100, 1000)
~~~
{: .language-python}

challenge 3:
~~~
mu = 0   # the position of the center of the peak
sigma = 10  # # The width of the 'bell'
g = gauss(x, mu, sigma)
~~~
{: .language-python}

challenge 4:
~~~
plt.plot(x, g)
plt.show()
~~~
{: .language-python}


## Noisy Signal

Now we are going to add some random noise to that curve. To do it we can use the numpy function normal from the module random provided by numpy library:

~~~
g = 100*g   # To have something visible we are multiplying the function by 100.

# Creation of the noise
noisy = np.random.normal(g)
plt.plot(x, g+noisy)
plt.show()
~~~
{: .language-python}

A way to calculate the 'Signal to Noise' ratio of the previous data set is by dividing the noisy data by the standard deviation of the noisy data.
~~~
# Get standard dev of noisy data
rms = np.std(noisy)
# Divide noisy data by std dev
SN = noisy / rms
# Plot
plt.plot(SN)
plt.show()
~~~
{: .language-python}

We will next create a mask that masks the data where the SN (signal to noise) is less than 1)
~~~
mask = SN < 1
print('Signal to noise:',SN[:10])
print('Mask:',mask[:10])
print('Mask shape:',mask.shape)
noisy_ma = np.ma.array(noisy, mask=mask)
plt.plot(noisy_ma)
plt.show()
~~~
{: .language-python}

## Working with images

We are going to learn some commands that deal with images. Since most scientific domains use their own file format, we obviously can not learn all of them. We will use a typical astronomical image format: the 'fits' file.

In the data directory you should find a file called 502nmos.fits.

We can verify that the file is indeed here:

~~~
cd data
ls *.fits
~~~
{: .language-python}


{% include links.md %}

















