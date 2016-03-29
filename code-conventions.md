# Code/Development Reference

A document for coding and other development related reference, including coding style and other conventions.

### Code Style: PEP8 as a soft standard
[PEP 0008] (https://www.python.org/dev/peps/pep-0008/) is the official Python coding style standard, with a lot of nice explanations and examples.
Basically we do not have to get into too many details, while some rules of development like required comments and naming conventions should be defined.

### Developing Tools: Anything convenient, monitored with git, interaged in .ipynb at last.
Whatever, text editors, IDE, Jupyter/IPython Notebook, anything. 
Note: the potential to integrate to .ipynb may be considered, which is actually very easy so no concerns in practice.

### Teamwork: via git/github for most of the time

### First Rule: readable code with doctrings and detailed comments

We both strongly agree that readablity is so important that it has everything to do with our developing efficiency. Thus,

#### for file/library/package/module: lowercase name with maybe underscore
''Modules should have short, all-lowercase names. Underscores can be used in the module name if it improves readability.''

#### for scripts: sections
We define sections in a script to demonstrate what to do, how to do and maybe why to do so.
For example, if in Spyder, we can do 
```python
# %% section: process data
...
# %% section: ...
...
```
#### for functions: naming, doctrings and comments
We name a function with lower case letters (and maybe numbers) with underscores as in PEP8.
```python
def get_transpose(matrix):
    ...
    return matrix_transpose
```
We add neccessary doctrings to demonstrate a function: what to do, how to do and perhaps why to do so.
```python
def function(x, y):
    '''This function does ...
    input x: <type>, what it is
    input y: <type>, what it is
    output r: <type>, what it is
    '''
    ...
    return r
```
We add neccessary detailed inline comments as reference in a function.
```python
def function(x, y):
    ...
    var_a = np.zeros(x) # set vector size instead of list.append() to improve efficiency
    ...
```
#### for .ipynb: add Markdown cells with comments or demonstartions.


### Manageble Files/Functions: one function for one use/goal
We try to define a function to implement one goal, instead of integrate several goals in one function. For example, if we are going to do 
```python
def get_std(data):
    '''
    output std: np.double, scalar, sample data standard deviation
    '''
    sample_mean = get_mean(data) # of course, most of time we use np.mean(data) here. Just trying to use this as an example.
    ...
    return std
```
instead of
```python
def get_std(data);
    ...
    sample_mean = sum(data)/len(data)
    ...
    return std
```

### Indent: only spaces and 4 spaces for a tab



