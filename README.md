# RFigure version 3

## Forewords

RFigure is a program that deals save in a specific file (whose extension is
rfig3) the data and the code that is needed to produce a matplotlib figure.
When creating a rfig3 file, is saved separately the data to display and the
instructions executed to display.

This code is written by R. Dessalles (Grumpfou) for the most part (the only
exceptions are the code in REditors that are adapted from example on the web).
It is proposed under the license GNU General Public License v3.0 (unless some
part of the code when specified otherwise,written by somebody else). It is
written in Python 3.6.6.

## Install

In a terminal, go in the same folder in which the sources have been installed, and use the following command lines:

```
pip install pyqt5
python setup.py install
```
You need to install PyQt5 independently because it can not be directly be
installed using setuptools of Python. (cf <https://stackoverflow.com/questions/4628519/is-it-possible-to-require-pyqt-from-setuptools-setup-py>).

Alternately, you can explicitly save in you user directories using:
```
pip install --user pyqt5
python setup.py install --user
```

***Note:*** It is possible by that using the ```--user``` option, the script
**rfig** is not saved in a directory which is is in the path. In that case,
be sure to add the directory that contains **rfig** (usually ~/.local/bin/) to
the path manually (see <https://askubuntu.com/questions/799302/ubuntu-cant-find-an-executable-file-in-local-bin>)

## Quick Example

Open Python terminal and try:
```
import RFigure,numpy
X = numpy.arange(0,10,0.1)
Y = numpy.cos(X)
i = "plot(X,Y)" # the instrucutions
d = dict(X=X,Y=Y) # the data to display
c = "This is a test" # the commentaries associate with the figures
rf = RFigure.RFigureCore(d=d,i=i,c=c)
rf.show() # execute the instrucutions to be sure it works
rf.save(filepath='./Test.rfig3') # only save the rfig3 file
rf.save(filepath='./Test.rfig3',fig_type='pdf') # save the rfig3 file as well as the pdf associated
```
Once a rfig3 file is saved, one can use the graphical interface to modify the
instructions using the script **rfig**:
> $ rfig ./Test.rfig3

![](./ExampleGui.png)

## How the code is organized

- RFigure3: contains the main two classes (RFigureCore and RFigureGui) used to
handle rfig3 files. RFigureCore is a direct tool used in the scripts to create
the files. RFigureGui is a gui interface used to modify a posteriori the
instructions
- RFigureConfig: contains the header that will be execute before any
instructions. It typically import numpy and matplotlib as the magic function
%pyplot does in Jupyter/IPython
- RPickle2: handels the coding and decoding of information contained in rfig3
files. Works in the same way as the regular pickle library of python
- REditors: contains the Syntax Highlighters used to display the python code
(for the instructions) and the markdown code (for the commentaries).

## Miscelaneous
- By default, numpy and pyplot are already imported when launching when
executing the instructions (similar to the magic function %pyplot does in
Jupyter/IPython)
- Local Header: When the program detect a file with the name
.RFigureHeaderLocal.py in the same dir as the file, it adds it to the header
(can be used to specify the font for all the rfig3 of the directory for instance).
- Format name: when set to true when save (or push the button in the gui
  interface), it will format the name of the figure as Figure_YYYYMMDD_foo.rfig3
  (where YYYYMMDD stands for the current date).
- Shortcurts in the graphical interface:
    - Ctrl+S will save the file;
    - Ctrl+M or Ctrl+Enter will show the figure.
    - Ctrl+/ will comment/uncomment the block
    - Ctrl+↑ will move line(s) up
    - Ctrl+↓ will move line(s) down
    - Ctrl+Shift+D will duplicate the line(s)
    - Ctrl+Shift+K will delete the line(s)
- SF_INSTRUCTIONS command: when the instructions in input contained at some
point the line
> \#! SF_INSTRUCTIONS

the program consider the instruction begins only at this point. It is useful
when using it with a Jupyter notebook:
```
**In[]:**
X = numpy.arange(0,10,0.1)
d=dict(X=X)
rf = RFigure3.RFIgureCore(i=In[-1],d=d)
rf.save(filepath='./Test.rfig3')
#! SF_INSTRUCTIONS
X=arange(0,10,0.1)
plot(X,cos(X))
```
Will save figure whose instructions are the last lines of the cell.

## FAQ

- Why is it RFigure3 (and not 2 or 1) and RPickle2 (and not 1)?
  I had several personal versions of these libraries.
- Why there is a 'R' in front of the modules?
  Because my first name is **R**enaud
- Why do not use the default pickle library of Python ?
  Because there is so much problem of compatibilities between the different
  versions of Python on this module. I better control the data with my own
  Pickle (the inconvenient is that it only deal with basic types)
- The logo is under [Creative Commons Zero 1.0 Public Domain License](https://creativecommons.org/publicdomain/zero/1.0/) and available at https://openclipart.org/detail/172389/graph
