# Collaboration

## Syncing with a remote
If you have cloned your repository from the cloud, then you can sync it using
``` bash
git push origin main
```
where origin is the name of the remote repository and main is the name of the current branch. If someone else has pushed a change to the repository, then git won't let you push. You'll first have to pull down their changes.
``` bash
git pull origin main
```


## Resolving Conflicts
If their changes conflict with your changes (e.g. you both changed the same line of the same file), then git will mark those conflicts with the following syntax
```
<<<<<<<<<
your code
=========
their code
>>>>>>>>>
```

so  you'll need to search through the files to find the "<<<<<" and replace that whole section with what you want the code to be.  This is called Resolving a Conflict. 

You can find the files which contain conflicts (and the line numbers) using
``` bash
git diff --check
```

## Practice
It is a good idea to practice some of these skills!
We can have each team pick a captain, the captain creates a repository and adds team members
The team members all clone down the team project
Then have everyone practice editing the same document and resolving conflicts..

# Automated Testing with pytest
## References:
* read [pytest documentation](https://docs.pytest.org/en/7.2.x/)
* read [Python testing with Pytest by Brian Okken](https://learning.oreilly.com/library/view/python-testing-with/9781680502848/)

## Overview
The key idea is to create files of the form test_ZZZZ.py which pytest will find and run.

Tests consist of 4 steps:

setup the environment (using fixtures)
call some functions or methods (using assertions)
test the results -- this is done with the "assert" statement
tear down the environment

We'll also show how you can "mark" some of the tests and the only run the tests with particular marks.

We start by looking at the [Getting Started with pytest page](https://docs.pytest.org/en/7.2.x/)

You need to install pytest with
``` bash
pip3 install pytest
```
and then create files of the form test_ZZZZ.py containing tests.
You run the tests using
``` bash
pytest -v
```
which will look for all files with the test_ prefix and run them.

Here is an example of a test file for the Quaternion.py example we saw earlier
``` bash
from quaternion import Quaternion
import pytest

def test_repr():
    ''' constructor should create a printable object '''
    t_quat = Quaternion(1,2,3,4)
    expected = 'Quaternion(1, 2, 3, 4)'
    assert t_quat.__repr__()==expected

def test_add():
    q1 = Quaternion(1,2,3,4)
    q2 = Quaternion(2,4,6,8)
    q3 = Quaternion(3,6,9,12)
    assert q1+q2==q3

def test_mul():
    q1 = Quaternion(0,1,2,3)
    q2 = Quaternion(0,-1,-2,-3)
    expected =Quaternion(14,0,0,0)
    assert q1.conjugate()==q2
    assert q1*q2==expected

def test_to_dict():
    q1 = Quaternion(2,7,1,8)
    expected = {'w':2,'x':7,'y':1,'z':8}
    assert q1.to_dict()==expected

def test_div_by_zero():
    q1 = Quaternion(1,0,0,0)
    q0 = Quaternion(0,0,0,0)
    with pytest.raises(Exception):
        q = q1/q0

def test_normalize_zero():
    q0 = Quaternion(0,0,0,0)
    with pytest.raises(Exception):
        q = q0.normalize()

```
There are two key ideas here -- assert and pytest.raises
pytest will use the assert statements to see if the code passes or fails the test
Likewise, it uses pytest.raises(E) to see if the code properly raises the exception E when run

