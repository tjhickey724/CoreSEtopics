# Lesson 8 - Intro to Python for Java Programmers

Today we do a quick review of Python for Java Programmers.

Here is a nice online intro to [Python for Java Programmers](http://python4java.necaise.org/Main/TableOfContents)
(We will mostly follow the main structure of this intro).

Here is the [Official Python tutorial](https://docs.python.org/3/tutorial/) which you should skim
and here is [Google's short Python class](https://developers.google.com/edu/python) if you want another intro.
and here is a [Python Cheat Sheet for Java Developers](https://github.com/akashp1712/awesome-python-cheatsheets#1-python-for-java-developers-1)

Today, we will go over the main differences between Python and Java and give you practice writing Python code.
Tomorrow, we'll show how to create and use Classes in Python.

## Statement syntax and semicolons
Java requires semicolons to terminate statements, but allows statements to span many lines.
Python requires semicolons to separate statements on the same line 
but doesn't require semicolons if statements are on separate lines.
Here is an example... but if you remove the semicolon after print(3) it will generate a syntax error
``` python
print(1) 
print(2); 
print(3); print(4)
print(5); print(6);
```

## Comments
Python uses the hashtag to indicate that the rest of a line is a comment
``` python
print('hello')  # this is a comment
```

## Statement blocks are indicated using indentation
This is the biggest syntactical difference between Python and other OOP and imperative languages.
Java uses curly braces {...} to indicate statement blocks such as while bodies, and if-then-else blocks.
Python uses a "colon" (:) at the end of the enclosing statement 
and [indentation](https://docs.python.org/3/reference/lexical_analysis.html#indentation) afterwards (typically a tab or 4 spaces).
``` python
for i in [2,3,5,7,11]:  # for loops can iterate over a list of Python values
    print('i=',i)
    if i%2==0:
        print(i,'is an even prime') # strings can use either ' or "
    else:
        print(i, "is an odd prime") 
    print('-'*10)  # you can multiply a string by an integer to repeat it
print('bye')
```

## identifiers
[Python identifiers](https://docs.python.org/3/reference/lexical_analysis.html#keywords) are similar to Java. They are case sensitive, can be of any length, and
consist of letters, digits, and underscores, starting with a letter or underscore.

We usually write multiword identifiers using an underscore between words (SnakeCase)
rather than capitalizing each word as we do in Java (CamelCase).

Also, we don't start an identifier with underscore except in certain special cases that
often occur when defining Classes.  Double underscore prefixes are usually reserved for 
Python internal variables and should be avoided.

Python 3.11 does not allow the following key words to be identifiers:
``` python
False      await      else       import     pass
None       break      except     in         raise
True       class      finally    is         return
and        continue   for        lambda     try
as         def        from       nonlocal   while
assert     del        global     not        with
async      elif       if         or         yield
```

## Functions
Python functions are defined using the def keyword 
``` python
def functionname(arg1, arg2, ...):
   '''  doc string comment describing what the function does
        this can span multiple lines
   '''
   statement1
   statement2
   ....
   statementn
   return(expression)  # return statements are optional, and NONE will be return if no return is given
```
Here is an example:
``` python
'''
    demo04 is a module 
    that contains a function is_prime(n) 
    that returns True if n is a positive prime, False otherwise
    author: Tim Hickey
    date: 2023-01-31
    .... programs should have a triple quote multiline comment at the top
    .... to explain what the code does, you can access this documentation with the help function
    .... the functions should also have a doc string right after the definition line,
    .... it can also be accessed in Python with the command help(is_prime)
'''

def is_prime(n):
    ''' returns True if n is a positive prime, False otherwise 
    '''
    for i in range(2,n):  # range(start,stop) generates the integers from start up to but not including stop
        if n%i== 0:
            return False  # if n is divisible by 2<=i<n it is not prime
        elif i*i>n:
            return True  # any proper divisor must be less than sqrt(n)
    return n>=2  # if n is 2 we never enter the loop

if __name__ == '__main__':  # this only runs if this file is run directly, not if imported as a module
    print('the primes < 100 are')
    for i in range(-3,100):
        if is_prime(i):
            print(i, end=', ')  #don't add a newline after printing, add a comma space instead
```

## Modules
A [Python module](https://docs.python.org/3/reference/import.html) is a file that contains Python code.
You can "import" a module into your program using the import statement.
For example, if the example above was in the file demo04.py in the same folder
as the following demo05.py program. We could import it and call it as follows:
``` python
``` demo05 shows how to import a module ```
import demo04

def main():
    ''' testing out is_prime '''
    num = input("enter a number: ") # get a number from the user
    n = int(num) # convert it to an integer
    if demo04.is_prime(n): # call the is_prime function from demo04
        print(n,'is prime!')
    else:
        print(n,'is not a positive prime number')

main()  # it is better to have all code inside functions, except the call to the main function 
```

We can also import just a single function from a module:
``` python
from demo04 import is_prime
```
in which case we can just call is_prime rather than demo04.is_prime

We can also import all functions directly without needing the demo04 prefix using the wildcard *
``` python
from demo04 import *
```

## Standard Packages
Python comes with a large set of very useful standardized modules compiled into packages
which you can import just using their name, e.g. the math module, the datetime module, the string module
These are all part of the [Python Standard Library](https://docs.python.org/3/library/index.html)

You can also access the 450,000+ packages at the [pypl.org](pypl.org) site, though some of these
might contain malware.

and, of course, you can write your own modules. 

## Data Structures
Python has essentially the same primitive data types as Java, but integers can be any length, and Python also allows complex numbers (1 + 2 J) where J is the imaginary number rather than the familiar "i"

Python also has a more expansive collection of compound data structures including lists, sets, tuples, and dictionaries, as well as control structures for iterating over those data structures

### Lists
Lists are indicated using square brackets and can have elements of any type
``` python
vals=[1,3.14159,'apple pie', True, None]
print(vals)
print('the 3rd element is',vals[2])
```
Python also allows one to access lists using "slice" notation vals[lo:hi] to get 
the sublist with indices from lo upto but not including hi
``` python
sentence = "a rose is a rose is a rose"
words = sentence.split(' ') #we can split a sentence into a list of words by a separator
print(words)
letters = list(sentence) #list turns a string into a list of words
print(letters)
words_again = "".join(letters) # sep.join(strlist) joins a list of strings using the separator sep
print(words_again)
repeater = words[2:5] # slice notation pulls out a sublist
print(repeater)
roses = words[1::3] # a third slice parameter is the step size, any parameter can be omitted for its default
print(roses)
print(words[::-1])  # step size of -1 reverses a list
```

# tuples
A tuple is like a list but is formed with parentheses (...) instead of square brackets [....]
and it is immutable, meaning you cannot change the elements of a tuple. 

# sets
A set is like a list but it contains only immutable objects with no duplicates and is created using
curly braces {...}

# conversion
You can convert between these sequence types using the functions list, tuple, and set
``` python
a_list = [1,2,'a','b',2,(2,3,4),'a','1']
a_tuple = (4,2,'x',[1,2,3])
a_set ={2,1,'b','a',(4,'c'),'a','b','a'}
print(a_list)
print(tuple(a_list))
print(set(a_list))
print(list(a_tuple))
print(list(a_set))
```

## Dictionaries
Dictionaries are key/value stores like HashMaps in Java. They are created with curly braces
enclosing key:value pairs separated by commas:
``` python
d1 = {'name':'Tim Hickey', 'age':67, 'gender':'M'}
print(d1)
print(d1['age')
for key in d1:
    print(key,d1[key])
```
