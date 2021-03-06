# RV_PS2017

Jupyter Notebooks for the **radial velocities** tutorial at the Precision Spectroscopy Workshop 2017, held at the *Instituto de Astronomia, Geofísica e Ciências Atmosféricas* (*Universidade de São Paulo*) in August 2017. If you have suggestions or corrections, just [open a GitHub issue](https://github.com/RogueAstro/RV_PS2017/issues).

Summary of this README:
1. [Requirements](https://github.com/RogueAstro/RV_PS2017#1-requirements)
2. [Setup](https://github.com/RogueAstro/RV_PS2017#2-setup)
3. [Technical background](https://github.com/RogueAstro/RV_PS2017#3-technical-background)
4. [Known issues](https://github.com/RogueAstro/RV_PS2017#4-known-issues)

### 1. Requirements

In order to follow this tutorial, it is necessary to have basic knowledge of command line in Unix-based systems. Having experience with Jupyter notebooks (a.k.a. IPython notebooks) and knowing how to use ``git`` and ``conda`` is recommended. The Python packages ``pandas``, ``radvel`` and ``radial`` (and their dependencies) need to be installed (see item 2). Basic knowledge of radial velocities and orbital parameters is also recommended (see item 3).

Also, make sure that ``pdflatex`` is installed. You can get ``pdflatex`` by installing the TexLive package or other LaTeX distributions. By default it is expected to be in your system’s path, but you may specify a path to ``pdflatex`` using the ``--latex-compiler`` option at the ``radvel`` report step.

### 2. Setup

We highly recommend using the [Anaconda](https://www.continuum.io/downloads) ecosystem, since it allows us to use the ``conda`` Python package and environment manager. Additionally, we recommend downloading the Python 3 version of Anaconda -- but do not worry about Python 2 codes, because we can easily setup a Python 2 environment using ``conda``.

For the radial velocities tutorials, we will create a Python 2.7 environment called ``rv``, and install the packages ``numpy``, ``scipy``, ``cython``, ``astropy``, ``pandas`` and ``matplotlib`` right from the start:

```
conda create -n rv python=2.7 numpy scipy cython astropy matplotlib pandas
```

Whenever you want to work in the ``rv`` environment, just issue the following command in the terminal:

```
source activate rv
```

And when you want to leave the environment, just use the following command:

```
source deactivate
```

Now, let's activate the ``rv`` environment and install some additional packages using the command ``pip``:

```
source activate rv
pip install emcee corner lmfit pp jupyter
```

**Note**: ``jupyter`` can also be install using the command ``conda install jupyter``.

Now we will proceed to install the radial radial velocities fitting codes. [``radvel``](http://radvel.readthedocs.io/en/master/index.html) is one of the RV packages we will use in this tutorial. It is authored by B. J. Fulton and E. Petigura. Unfortunately it works only in Python 2, which is why we setup the ``rv`` environment with ``python=2.7``. The other package we will use in this tutorial is [``radial``](https://github.com/RogueAstro/radial), which is authored by L. dos Santos (it works in both Python 2 and 3, but it's optimized for Python 3).

In order to install ``radial``, go to the terminal and navigate to the folder you want to save the source code and issue the commands:

```
git clone https://github.com/RogueAstro/radial.git
cd radial
python setup.py develop
```

To install ``radvel``, I recommend compiling the source code from the authors repository. In the terminal, navigate to the folder you want to save the source code and:

```
git clone https://github.com/California-Planet-Search/radvel.git
cd radvel
python setup.py develop
```

You can also clone the [version of ``radvel`` that I forked](https://github.com/RogueAstro/radvel) from the original repository. I plan on modifying ``radvel`` in the near future to work with Python 3 and possibly adding some features here and there. Another option is to use ``pip install radvel``, but I do not recommend it.

### 3. Technical background

The basic notions of radial velocities for binary stars and exoplanets can be found in chapter 2 of the book [Exoplanets by Sara Seager](http://seagerexoplanets.mit.edu/books.htm). This chapter is authored by C. D. Murray and A. C. M. Correia, and it is freely [available on arXiv](http://arxiv.org/abs/1009.1738).

Further reading for the enthusiasts:
* Minimum masses from minimal RV variation: [Torres (1999)](http://iopscience.iop.org/article/10.1086/316313), [Feng et al. (2015)](http://stacks.iop.org/0004-637X/800/i=1/a=22?key=crossref.07b640baf68c8ce0b4ded8aef8aa6074), [Jenkins et al. (2015)](http://arxiv.org/abs/1507.04749)
* Alpha Cen Bb does not exist: [Rajpaul et al. (2015)](http://arxiv.org/abs/1510.05598)
* The radial velocity fitting challenge: [Dumusque (2016)](http://arxiv.org/abs/1607.06487)
* Systemic Console: [Meschiari et al. (2009)](https://arxiv.org/abs/0907.1675)
* EXOFAST: [Eastman et al. (2013)](https://arxiv.org/abs/1206.5798)
* Markov Chain Monte Carlo techniques: [Ford (2005)](http://stacks.iop.org/1538-3881/129/i=3/a=1706)
* [Basic ``emcee`` example](http://dan.iel.fm/emcee/current/user/line/)
* [Fitting a plane to data](http://dan.iel.fm/posts/fitting-a-plane/)
* [A practical guide to the Lomb-Scargle Periodogram](http://jakevdp.github.io/blog/2017/03/30/practical-lomb-scargle/)
* [Frequentism vs. Bayesianism](http://jakevdp.github.io/blog/2014/03/11/frequentism-and-bayesianism-a-practical-intro/) (Spoiler: Frequentism sucks)
* [Toolkit for planet detection and characterization](https://reddots.space/toolkit/)

### 4. Known issues

#### 4.1. Compilation of C codes fails

You may run into problems when installing Python packages that compile C code with Anaconda. If the installation of the packages ``jupyter`` or ``radvel`` fails, use a custom installation of ``gcc``. First, activate the ``rv`` environment if is not activated yet, and install the custom ``gcc``:
```
source activate rv
conda install -c asmeurer gcc=4.8.5
```
This will install the custom ``gcc`` in the ``rv`` environment, but will leave the original installation of ``gcc`` untouched outside of this specific environment.

#### 4.2. Jupyter notebooks do not initialize the right kernel

Have you tried turning it off and on again? No, seriously, if you installed ``jupyter`` inside the ``rv`` environment, you need to deactivate and then reactivate the ``rv`` environment again in order to initialize the correct kernel.

#### 4.3. I get a ``NameError`` when running MCMC

When executing the ``mcmc`` part of ``radvel``, you may run into the following error:

```
NameError: global name 'Pool' is not defined
```

To be honest, I don't exactly know what causes this issue at the moment. I got it in my Mac, but not in my Linux workstation, even though I followed the same setup in both machines. The only solutions I found is to either compile ``radvel`` from the source or install the Python 2 version of Anaconda and use that as the package manager.
