---
title: "Software Package Management"
teaching: 0
exercises: 0
questions:
- "How can I load new python libraries?"
- "How can I deal with libraries which have competing software requirements"
- "How can I record the python libraries that I use for my work?"
objectives:
- "Learn how to install a conda package"
- "Learn how to use different conda channels"
- "Learn how to create a new conda environment"
- "Learn how to activate / deactivate conda environments"
- "Learn how to record the setup of a conda environment"
keypoints:
- "`conda install`"
- "`conda env create`"
- "`conda activate`"
- "`conda deactivate`"
- "`conda save`"
---

Virtual environments are a useful tool for isolating and managing the software packages you use. Doing this enables you to track the packages you use for your work (enabling you to create reproducable environments, so that others can also use your code). It also allows you to use packages which might have conflicting requirements (or even different versions of the same package) without the hassle of haveing to install and uninstall these each time.

Python has a built in tools for managing virtual environments ([venv](https://docs.python.org/3/tutorial/venv.html)) and packages (pip). However we will not cover these tools today, While venv and pip are very useful for managing pure python packages, often we want to use packages which are built on other languages (or, even, are do not use python at all), and they are not very flexible in such a situation. Another tool, conda, has been built to extend their functionality to cover these mixed-language packages, for a wide range of computing platforms, and it is this which what we will cover today.

The lesson below is not conducted in a python interpreter, but instead in the unix shell. When you are asked to type in code below, please do this in a terminal window, not in this browser.

## Installing Conda

The conda package manager is available to install by itself (using the miniconda installer), or with a preprepared set of commonly used packages and a graphical user-interface (GUI) for managing your packages (using the anaconda installer). Miniconda is a good choice if you have limited bandwidth or disk space, and are happy to install packages as you need them using the command line interface (CLI). Anaconda is a good choice if you have more disk space (it requires >3Gb) and want to use a GUI.

We will not cover installation in this lession, but more information is available on both of these, as well as links for downloading the installers, is available here: [https://docs.conda.io/projects/conda/en/latest/user-guide/install/download.html](https://docs.conda.io/projects/conda/en/latest/user-guide/install/download.html).

## Package Channels

Conda uses "channels", locations where software packages are stored. Conda has a default set of these channels that it searches when you request a package or update. You can modify this list of channels, to add other public or private channels that you wish to use. One of the most commonly added is [conda-forge](https://conda-forge.org/), a community channel with thousands of contributors who use its unified, automated build infrastructure to share their software with the world.

We can see what channels are being used by the command:
~~~
conda config --show channels
~~~
{: .language-bash}

This will (likely) list these as:
~~~
channels:
  - conda-forge
  - defaults
~~~
{: .output}

## Searching for Packages

~~~
conda search 'spyder>=4.2'
~~~
{: .language-bash}


~~~
conda search perl*
~~~
{: .language-bash}


## Creating Environments

As the range and variety of software used increases, it is becoming very common for users to need to use a number of different software programs for their work which need access to different versions of the same software library. Or there will be conflicts between the operating system requirements (as these tend to change slowly, and be built on reliable software libraries) and the latest version of the software package you need. Usually installing all this software on the same system would be very difficult - but if we use virtual environments to separate these programs and their dependencies, then the task becomes a lot more simple.

We will start with listing the current environments:
~~~
conda env list
~~~
{: .language-bash}
~~~
# conda environments:
#
base                  *  /Users/mbessdl2/anaconda3
~~~
{: .output}
This shows you the environments you have installed, the path where they are installed, and indicates which environment you currently have loaded.

A new conda environment can be created using the command:
~~~
conda create --name myenv
~~~
{: .language-bash}
This will create an environment named myenv (once you press `[y]` to proceed). Check this using the `conda env list` command.
~~~
# conda environments:
#
base                  *  /Users/mbessdl2/anaconda3
myenv                    /Users/mbessdl2/anaconda3/envs/myenv
~~~
{: .output}
This environment will be empty of all packages though (you will find out later how to check this).

We can also specify the packages we want to install when creating an environment. When this is done conda will work out all the necessary supporting packages for you. For example:
~~~
conda create --name myenv spyder
~~~
{: .language-bash}
This will list the packages which are to be downloaded, and those which will be installed, before asking if you wish to continue. Press `[y]` to continue, and then check that the environment exists using conda env list as before.

## Activating and Deactivating Environments

Now that we have installed `spyder` we would like to run it. However it is not available to us yet:
~~~
which spyder
~~~
{: .language-bash}
(if this cannot find the `spyder` program it will return nothing)

Before we can use this software, we must activate the environment it is in. This can be done using:
~~~
conda activate myenv
~~~
{: .language-bash}
or (if your shell has not been configured to use conda activate)
~~~
. activate myenv
~~~
{: .language-bash}
Once this done you should see the name of the environment you are using in brackets before your command prompt. And you will now be able to access the program:
~~~
which spyder
~~~
{: .language-bash}
~~~
/Users/mbessdl2/anaconda3/envs/myenv/bin/spyder
~~~
{: .output}





{% include links.md %}

