---
title: Software Package Management
teaching: 0
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Learn how to install a conda package
- Learn how to use different conda channels
- Learn how to create a new conda environment
- Learn how to activate / deactivate conda environments
- Learn how to record the setup of a conda environment

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I load new python libraries?
- How can I deal with libraries which have competing software requirements
- How can I record the python libraries that I use for my work?

::::::::::::::::::::::::::::::::::::::::::::::::::

Virtual environments are a useful tool for isolating and managing the software packages you use. Doing this enables you to track the packages you use for your work (enabling you to create reproducable environments, so that others can also use your code). It also allows you to use packages which might have conflicting requirements (or even different versions of the same package) without the hassle of haveing to install and uninstall these each time.

Python has a built in tools for managing virtual environments ([venv](https://docs.python.org/3/tutorial/venv.html)) and packages ([pip](https://pypi.org/project/pip/)). However we will not cover these tools today. While venv and pip are very useful for managing pure python packages, they are not very flexible when we want to use packages which are built on other languages (or perhaps do not use python at all). Another tool, conda, has been built to extend their functionality to cover these mixed-language packages, for a wide range of computing platforms, and it is this which we will cover today. More details on the differences between conda and pip are given in this [summary](https://www.anaconda.com/blog/understanding-conda-and-pip).

The lesson below is not conducted using a python interpreter, but instead using the unix shell. When you are asked to type in code below, please do this in a terminal window and not in this browser.

## Installing Conda

The conda package manager is available to install by itself (using the miniconda installer), or with a preprepared set of commonly used packages and a graphical user-interface (GUI) for managing your packages (using the anaconda installer). Miniconda is a good choice if you have limited bandwidth or disk space, and are happy to install packages as you need them using the command line interface (CLI). Anaconda is a good choice if you have more disk space (it requires >3Gb) and want to use a GUI.

We will not cover installation in this lesson but more information is available on both of these conda installers, as well as links for downloading the installers, here: <https://docs.conda.io/projects/conda/en/latest/user-guide/install/download.html>.

## Package Channels

Conda uses "channels", locations where software packages are stored. Conda has a default set of these channels that it searches when you request a package or update. You can modify this list of channels, to add other public or private channels that you wish to use. One of the most commonly added is [conda-forge](https://conda-forge.org/), a community channel with thousands of contributors who use its unified, automated build infrastructure to share their software with the world. Another widely-used channel is [bioconda](https://bioconda.github.io/), which is dedicated to bioinformatics software.

We can see what channels are being used by the command:

```bash
conda config --show channels
```

This will (likely) list the the following:

```output
channels:
  - defaults
```

The channel order is important for determining priority when installing packages. Those from channels higher in the list will be given priority over those lower in the list. After this the newer versions of a package are given priority over older versions of a package (and then build numbers, for a given version). The order in which priority is determined can be changed, we won't cover this today, but more information can be found in the [conda managing channels](https://docs.conda.io/projects/conda/en/4.6.1/user-guide/tasks/manage-channels.html) documentation.

To add a channel we can use:

```bash
conda config --add channels conda-forge
conda config --show channels
```

```output
channels:
  - conda-forge
  - defaults
```

We can repeat this to add as many channels as we need:

```bash
conda config --add channels bioconda
conda config --show channels
```

```output
channels:
  - bioconda
  - conda-forge
  - defaults
```

This adds the new channel to the start of the list. Which gives that channel highest priority. The documentation for bioconda, however, recommends a priority order of conda-forge, bioconda, and finally defaults. To correct the channel order we can simply re-add the conda-forge channel:

```bash
conda config --add channels conda-forge
```

```output
Warning: 'conda-forge' already in 'channels' list, moving to the top
```

When we do this we should be warned the channel is already in the list, but it is being moved up to the highest priority position.

:::::::::::::::::::::::::::::::::::::::::  callout

## removing or appending channels

Channels can be removed using the `--remove` flag instead of `--add`. To add a channel
to the end of the list, rather than the front, we can use `--append` instead of `--add`
(and, if we wish to be explicit in our intent to put a channel at the front of the list
we can use `--prepend`, which behaves in the exact same way as `--add`).


::::::::::::::::::::::::::::::::::::::::::::::::::

## Searching for Packages

Once you've added the channels you wish to use, you can now search them for the software that you need. This is simply carried out using `conda search`. For example, if we wished to find out what versions of [Gromacs](https://www.gromacs.org/About_Gromacs) are available, we can use:

```bash
conda search gromacs
```

```output
# Name                       Version           Build  Channel
gromacs                        4.6.5               0  bioconda
gromacs                       2018.3      h470a237_0  bioconda
gromacs                       2018.4      h470a237_0  bioconda
gromacs                       2018.5      h04f5b5a_0  bioconda
gromacs                       2018.6      h04f5b5a_0  bioconda
gromacs                       2018.6      h04f5b5a_1  bioconda
gromacs                       2018.6      hd895feb_2  bioconda
gromacs                         2019      h04f5b5a_0  bioconda
gromacs                       2019.1      h04f5b5a_0  bioconda
gromacs                       2019.1      h04f5b5a_1  bioconda
gromacs                       2019.1      h04f5b5a_2  bioconda
gromacs                       2020.5      hd895feb_1  bioconda
gromacs                       2020.5      hd895feb_2  bioconda
gromacs                       2020.5      hd895feb_3  bioconda
gromacs                       2020.5      hd895feb_4  bioconda
gromacs                       2020.5      hd895feb_5  bioconda
gromacs                       2020.6      h94ec9d8_0  bioconda
gromacs                         2021      hd895feb_0  bioconda
gromacs                         2021      hd895feb_1  bioconda
gromacs                       2021.1      hd895feb_0  bioconda
```

This shows you the name of the package you searched for, then the version of the software, build number (which can increment up as bugs are fixed, see above for version 2020.5), and the channel that the package is in.

If we know the version of the software we are interested in we can narrow this search down. For example, if we are only interested in gromacs versions greater than `2020` we can use:

```bash
conda search "gromacs>=2020"
```

```output
# Name                       Version           Build  Channel
gromacs                       2020.5      hd895feb_1  bioconda
gromacs                       2020.5      hd895feb_2  bioconda
gromacs                       2020.5      hd895feb_3  bioconda
gromacs                       2020.5      hd895feb_4  bioconda
gromacs                       2020.5      hd895feb_5  bioconda
gromacs                       2020.6      h94ec9d8_0  bioconda
gromacs                         2021      hd895feb_0  bioconda
gromacs                         2021      hd895feb_1  bioconda
gromacs                       2021.1      hd895feb_0  bioconda
```

Note that we've put the search string in quotations, in order to ensure it is passed to conda correctly.

We can also use wildcards in the version string. For example, if we wish to find all numpy packages with version 1.20:

```bash
conda search "numpy==1.20.*"
```

```output
# Name                       Version           Build  Channel
numpy                         1.20.0  py37h3795f5d_0  conda-forge
numpy                         1.20.0  py37ha9839cc_0  conda-forge
numpy                         1.20.0  py38h64deac9_0  conda-forge
numpy                         1.20.0  py39h3c955ea_0  conda-forge
numpy                         1.20.1  py37h3795f5d_0  conda-forge
numpy                         1.20.1  py37h43259c0_0  pkgs/main
numpy                         1.20.1  py37ha9839cc_0  conda-forge
numpy                         1.20.1  py37hd6e1bb9_0  pkgs/main
numpy                         1.20.1  py38h43259c0_0  pkgs/main
numpy                         1.20.1  py38h64deac9_0  conda-forge
numpy                         1.20.1  py38hd6e1bb9_0  pkgs/main
numpy                         1.20.1  py39h3c955ea_0  conda-forge
numpy                         1.20.1  py39h43259c0_0  pkgs/main
numpy                         1.20.1  py39hd6e1bb9_0  pkgs/main
numpy                         1.20.2  py37h84c02c4_0  conda-forge
numpy                         1.20.2  py37hc415c66_0  conda-forge
numpy                         1.20.2  py38had91d27_0  conda-forge
numpy                         1.20.2  py39h7eed0ac_0  conda-forge
```

Note that the build string now starts with `pyXX`, where `XX` indicates the version of python the package was built against. When conda packages are dependent on python then they usually will be built against a number of different versions of python - which gives more flexibility when trying to create an environment. Also note that two channels are listed here, the default `pkgs/main` and `conda-forge`. These duplicate some versions, but also note that `conda-forge` has the most recent version of the `numpy` software, whereas the default channel does not. `conda-forge` tends to be updated more frequently than the default channels.

## Creating Environments

As the range and variety of software used increases, it is becoming very common for users require a number of different software programs for their work which need access to different versions of the same software library. In addition there could be conflicts between the operating system requirements (as these tend to change slowly, and be built on reliable software libraries) and the latest version of the required software package. Usually, installing all this software on the same system would be very difficult, but if we use virtual environments to separate these programs and their dependencies, then the task becomes a lot more simple.

We will start with listing the current environments:

```bash
conda env list
```

```output
# conda environments:
#
base                  *  /Users/mbessdl2/anaconda3
```

This shows you the environments you have installed, and the path where they are installed. In addition the `*` indicates which environment you currently have loaded. In the example above, anaconda is installed by the user, and is only accessible by that particular user. However, if anaconda has been installed by the administrator of that computer then you might something like:

```output
# conda environments:
#
base                  *  /opt/apps/anaconda
```

In this case the anaconda install will be accessible for all users, however no-one other than the administrator will be able to install new packages into the `base` environment.

You can list the packages you have installed in your current environment using:

```bash
conda list
```

```output
# packages in environment at /Users/mbessdl2/anaconda3:
#
# Name                    Version                   Build  Channel
_anaconda_depends         2019.03                  py37_0
_ipyw_jlab_nb_ext_conf    0.1.0                    py37_0
alabaster                 0.7.12                     py_0    conda-forge
anaconda                  custom                   py37_1
anaconda-client           1.7.2                      py_0    conda-forge
anaconda-navigator        1.9.12                   py37_0
anaconda-project          0.8.3                      py_0    conda-forge
...
zip                       3.0                  h1de35cc_1    conda-forge
zipp                      3.1.0                      py_0    conda-forge
zlib                      1.2.11            h0b31af3_1006    conda-forge
zstd                      1.4.4                he7fca8b_1    conda-forge
```

When you begin using conda, as you can see, you are given a default environment named `base`. Avoid installing programs into your base environment, it is better to create separate environments to keep your programs isolated from each other. A new conda environment can be created using the command:

```bash
conda create --name myenv
```

This will create an environment named myenv (once you press `[y]` to proceed). Check this using the `conda env list` command.

```output
# conda environments:
#
base                  *  /Users/mbessdl2/anaconda3
myenv                    /Users/mbessdl2/anaconda3/envs/myenv
```

This environment will be empty of all packages though, as we can show by specifying the `myenv` environment when we list the installed packages:

```bash
conda list myenv
```

```output
# packages in environment at /Users/mbessdl2/anaconda3:
#
# Name                    Version                   Build  Channel
```

We can also specify the packages we want to install when creating an environment. When this is done conda will work out all the necessary supporting packages for you. For example, we will create the `myenv` environment again, this time installing the [spyder](https://www.spyder-ide.org/) IDE package, as well as the [pandas](https://pandas.pydata.org/) and [matplotlib](https://matplotlib.org/) packages, which we will need later:

```bash
conda create --name myenv spyder pandas matplotlib
```

This will warn you that you are about to overwrite another environment, press `[y]` to continue. Conda will then workout the new environment setup (which can take a little time), and then will list the packages which are to be downloaded, and those which will be installed, before asking if you wish to continue. Press `[y]` to continue, and then check that the environment exists and that the packages you expect are installed using `conda env list`, and `conda list myenv` as before.

## Activating and Deactivating Environments

Now that we have installed `spyder` we would like to run it. However it is not available to us yet:

```bash
which spyder
```

(if this cannot find the `spyder` program it will return nothing)

Before we can use this software, we must activate the environment it is in. This can be done using:

```bash
conda activate myenv
```

Once this done you should see the name of the environment you are using in brackets before your command prompt. (If you have used pip, this will be familiar.) And you will now be able to access the program:

```bash
which spyder
```

```output
/Users/mbessdl2/anaconda3/envs/myenv/bin/spyder
```

:::::::::::::::::::::::::::::::::::::::::  callout

## What if conda hasn't been configured yet?

It is possible that `conda activate <ENV>` will give you this error:

```output
CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
To initialize your shell, run

    $ conda init <SHELL_NAME>

Currently supported shells are:
  - bash
  - fish
  - tcsh
  - xonsh
  - zsh
  - powershell

See 'conda init --help' for more information and options.

IMPORTANT: You may need to close and restart your shell after running 'conda init'.
```

This will often happen on computers with a central installation of anaconda, where the
anaconda environment management tools will have not been initialised (as many users will
not need them). This can be corrected by following the instructions in the error
message (assuming that your current shell is `bash`, change the last word if not):

```bash
conda init bash
```

This will modify your personal shell configuration (in file `/home/mbessdl2/.bashrc` or
similar). You will have to close that shell, and then open another shell for the
changes to take effect.


::::::::::::::::::::::::::::::::::::::::::::::::::

## Installing Software into an Existing Environment

During your work you often will find that you need to add new software packages to your environment. For example, we will need the `astropy` and `pint` packages for the units and quantities lesson, and so we should add these now.

To install these packages (into the current environment we are in) we can use:

```bash
conda install astropy pint
```

If you are installing packages into an environment which is not active (or if you want to be certain that you are installing into the correct environment), you can add `-n <ENV>`:

```bash
conda install -n myenv astropy pint
```

Uninstalling software can be carried out in a similar manner, using `conda uninstall`.

## Recording and Reproducing Environment Setups

A big advantage of using virtual environments is the ability to record the environment in which you carry out your work, so that this can be shared with others in order to help them replicate your work.

The most basic way to obtain this information is using:

```bash
conda list --export
```

```output
# This file may be used to create an environment using:
# $ conda create --name <env> --file <this file>
# platform: linux-64
_libgcc_mutex=0.1=conda_forge
_openmp_mutex=4.5=1_gnu
alabaster=0.7.12=py_0
alsa-lib=1.2.3=h516909a_0
appdirs=1.4.4=pyh9f0ad1d_0
argh=0.26.2=pyh9f0ad1d_1002
arrow=1.1.0=pyhd8ed1ab_1
astroid=2.5.6=py39hf3d152e_0
...
zeromq=4.3.4=h9c3ff4c_0
zipp=3.4.1=pyhd8ed1ab_0
zlib=1.2.11=h516909a_1010
zstd=1.4.9=ha95c52a_0
```

This contains similar information to the standard `conda list` command, but in a machine readable formation. The output information can be saved to a text file using `conda list --export > envfile.txt`, which can then be used to recreate the environment elsewhere, using the command `conda create --name <env> --file <this file>`. However, it should be noted that this will be restricted to the operating system it is created on, and the channel information is not recorded in this output - so if you have used more than just the default and conda-forge channels it may be difficult for someone else to replicate.

A better method is to use:

```bash
conda env export
```

```output
name: myenv
channels:
  - conda-forge
  - bioconda
  - defaults
dependencies:
  - _libgcc_mutex=0.1=conda_forge
  - _openmp_mutex=4.5=1_gnu
  - alabaster=0.7.12=py_0
  - alsa-lib=1.2.3=h516909a_0
  - appdirs=1.4.4=pyh9f0ad1d_0
  - argh=0.26.2=pyh9f0ad1d_1002
  - arrow=1.1.0=pyhd8ed1ab_1
  - astroid=2.5.6=py39hf3d152e_0
 ...
  - zeromq=4.3.4=h9c3ff4c_0
  - zipp=3.4.1=pyhd8ed1ab_0
  - zlib=1.2.11=h516909a_1010
  - zstd=1.4.9=ha95c52a_0
prefix: /home/mbessdl2/.conda/envs/myenv
```

This will present the same information, but in the more readable [YAML](https://yaml.org/) format, and it will also include the channel information in the output. This information can be saved as a text file as before, and used to create a new environment using the command `conda env create`. However it is still operating system specific information.

To create a higher-level record of the environment you can use:

```bash
conda env export --from-history
```

```output
name: myenv
channels:
  - conda-forge
  - bioconda
  - defaults
dependencies:
  - pandas
  - spyder
  - astropy
  - pint
prefix: /home/mbessdl2/.conda/envs/myenv
```

This builds a list of only the packages that you explicitly requested to be installed in that environment. Where you have specified a particular version of a package, then this will be recorded, but otherwise the version number will be unconstrained. If you wish to constrain the version of a package (for example, if a certain feature you use has been removed in more recent versions of the package), then you will need to edit this information to specify that (using the information given by the standard `conda list` command).

To create a new environment from either of these YAML environment files, you can use the command:

```bash
conda env create --file <env file> --name <env name>
```

The name that you give the environment does not have to be the same as the name specified in the file, but if you don't explicitly specify a name then the name in the file will be used.



:::::::::::::::::::::::::::::::::::::::: keypoints

- conda virtual environments are useful for installing programs with differing requirements
- `conda config --add channels <channel>` adds a new channel to your list of sources
- `conda search <package>` will find all available versions of a package in your list of sources
- `conda create -n <env> <package(s)>` can be used to create a virtual environment from a list of packages
- `conda install -n <env> <pacakge(s)>` installs packages in a pre-existing environment
- `conda activate <env>` activates the named environment, giving access to the software installed there
- `conda deactivate` deactivates the current environment
- `conda env export --from-history > <file.yml>` creates a portable record of your current environment
- `conda env create --file <file.yml> <env>` creates a new environment from an environment file

::::::::::::::::::::::::::::::::::::::::::::::::::


