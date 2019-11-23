# Python Cheat Sheet

This is a really condensed cheat sheet to get introduced into python. This assumes we use **python 3.x**

## Basics

### Install

Windows / OSX: https://www.python.org/ 

Linux / WSL: `sudo apt update && sudo apt install python3 python3-pip`

> I recommend using python3 as python 2.x will [come into deprecation soon](https://pythonclock.org/)

### Run Python

Running Python interactive shell : `python3`

* Interactive shell allows you to input commands and test scripts without using a script file.

Running a Python script `$ python ./my_script.py`

Python scripts have the **.py** extension

### Installing more components using PIP

PIP is a package installer, it downloads and adds features to the current python installation

`$ pip install module1 module2` : installs modules

`$ pip install -u module2`: updates a module

> Note: On windows, PIP can't be called directly, it has to be called from `python3 -m pip`

## Writing Python Scripts

### Imports

Imports modules reside at the top of the script file.

`import module` : imports all the module classes

`from module import SomeClass, SomeOtherClass`: imports only some of the module classes

Many libraries exist in standard python installations, but more can be installed using **PIP**

## Python Syntax

Python is script language that is executed from top to bottom. every line contains one statement every statement has to terminate using a line return.

```python
print("This is a statement")
print("This is another statement")
name = "foo"
print("My name is {}".format(foo))
```

### Indentation

Indentation in Python defines the scope of the code: for instance anything indented below an **if** statement will be part of the **if condition scope**.
```python
if my_condition == True:
    # this instruction will be executed only if true
    print("this is true")
    # end if
# then, let's go back to our execution outside of the if
print("I always say that")
```

### Variables

Variables are data storage for values that can be then modified, assigned, read and compared to other variables. 

Variables can be assigned using the `=` equal operator (note that it is different from the `==` equality comparison operator)

Variables exist in the function scope where they are declared.

Variables are **dynamically typed** and their creation and assignment follows these rules:

```python
name = "foo" 			# a new variable and assigns it the string "foo"
surname = "bar" 	 	# another variable and assigns is the string "bar"
name = "foo fighter" 	# changes the name of the first variable
surname = name 			# assigns the contents of name into surname
name = 1 				# name changes type and now becomes an integer

def getYear(): 			# now we enter a function scope
    birth = 2019 		# we create birth variable into the scope
    print(birth) 		# prints '2019'
    #end function
    
print(birth) 			# ERROR! birth does not exist here, as is was created in
						# the getYear function scope
```


### 