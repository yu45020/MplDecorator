# MplDecorator

Python decorator for matplotlib and seaborn plots. Users only need to add one line on top of a plotting function to set pre-customized styles without changing current plot styles. The current version supports LaTex decorator.

The internal color scheme for multi-line plots is inherited from [Yi-Xin Liu's mpltex](https://github.com/liuyxpp/mpltex)'s color file and is modified to be compatible in Python 3.  The default scheme is Tableau classic 10 and can be changed to ColorBrewer Set 1 and Tableau classic 20 in the [color.py](https://github.com/yu45020/MplDecorator/blob/master/MplDecorator/colors.py) file.  For information on Tableau scheme can be found in [Palettable: Color palettes for Python](https://jiffyclub.github.io/palettable/). 

This tool is inspired by and built on
[Yi-Xin Liu's mpltex](https://github.com/liuyxpp/mpltex) and 
[masasin's latexipy](https://github.com/masasin/latexipy)

R users may want to check ggpubr to easily get publication-ready plots.


## Installing

In terminal, type 
```
    pip install git+https://github.com/yu45020/MplDecorator/
```
or. Download the package and unzip it. In terminal and the directory of the package, run 
    ```
    python setup.py install
    ```

## Known Issues

1. Must define a plot function to use the decorator.
2. In order to save in eps format, plots must be saved inside the plot function. Saving in PDF format will result in error (may be use.LaTex error)
3. To convert a eps file to PDF, insert them into a latex file and compile that file, or type the following code in python:

   
```python
import subprocess

subprocess.call(["epstopdf", "fig_name.eps"])
# Names with empty space is supported. A pdf with the same name will be created.
# To change file name, use the following:
# subprocess.call(["epstopdf", "--outfile", "outfig_name.pdf", "fig_name.eps"])
```

## Available Decorators

### @MplDecorator.latex_decorator

Parameters can be customized in [latex.py](https://github.com/yu45020/MplDecorator/blob/master/MplDecorator/latex.py): 
```
font size for all: 8
font family: serif
usetex: True
figure size: (width=4.296, height=2.655)
savefig format: eps
savefig.dpi: 900 
seaborn.style: 'seaborn-white' & "seaborn-paper"
plt.switch_backend: pgf (better for saving figs in eps and pdf, but no preview)
```

## Helper Function 

### Line Style Iterator

``` python
MplDecorator.linestyles(colors, lines, markers, hollow_styles, marker_size)
```
Default values are supplied. Marker_size is 0.5 by default. If don't want marker in lines, set markers = False. 

To modify this function, please check [styles.py](https://github.com/yu45020/MplDecorator/blob/master/MplDecorator/styles.py)

## Prerequisites
Python 3

Python 2 is not tested yet.


## Sample Usages

``` python 
import os, sys
sys.path.append(os.getcwd())
import MplDecorator
import matplotlib.pyplot as plt
import numpy as np



x = np.linspace(0, 3)
y = np.sin(x)
z = np.cos(x)

@MplDecorator.latex_decorator
def text_plot():
    fig,ax = plt.subplots()
    # fig, ax = plt.subplots(figsize=(11,11))
    line_style = MplDecorator.linestyles()
    next(line_style)
    ax.plot(x,y,label='y', **next(line_style))
    ax.plot(x,z, label='z', **next(line_style))
    ax.set_title("With Decorator")
    ax.set_xlabel('Some x')
    ax.set_ylabel('Some y')
    ax.legend(loc='best')
    plt.grid()
    fig.savefig("With Decorator.eps")

text_plot()
os.system('epstopdf "With Decorator.eps"')
```
<img src="https://user-images.githubusercontent.com/28139045/33397655-29ba6880-d501-11e7-9068-ad3b96f1981d.png" width="600">

```python
def text_plot2():
    #sns.lmplot(x="Time", y="value", hue="variable", data=dat)
    fig,ax = plt.subplots()
    line_style = MplDecorator.linestyles()
    next(line_style)
    ax.plot(x,y,label='y', **next(line_style))
    ax.plot(x,z, label='z', **next(line_style))
    ax.set_title("No Decorator")
    ax.set_xlabel('Some x')
    ax.set_ylabel('Some y')
    ax.legend(loc='best')
    fig.savefig("After Decorator.eps")

text_plot2()
os.system('epstopdf "After Decorator.eps"')
```

(Note: this is the default plot style, and its settings are not changed by the previous decorator)

<img src="https://user-images.githubusercontent.com/28139045/33397652-29557146-d501-11e7-9967-7fefa71fd639.png" width="600">

```python
@MplDecorator.latex_decorator
def text_plot_larger():
    fig,ax = plt.subplots(figsize=(6,6))
    # fig, ax = plt.subplots(figsize=(11,11))
    line_style = MplDecorator.linestyles()
    next(line_style)
    ax.plot(x,y,label='y', **next(line_style))
    ax.plot(x,z, label='z', **next(line_style))
    ax.set_title("Larger With Decorator")
    ax.set_xlabel('Some x')
    ax.set_ylabel('Some y')
    ax.legend(loc='best')
    plt.grid()
    fig.savefig("With Decorator Larger.eps")

text_plot_larger()
os.system('epstopdf "With Decorator Larger.eps"')
```

<img src="https://user-images.githubusercontent.com/28139045/33397653-299bd9f6-d501-11e7-8151-af068484866f.png" width="600">
