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

* setup the environment (using fixtures)
* call some functions or methods (using assertions)
* test the results -- this is done with the "assert" statement
* tear down the environment

We'll look at the setting up and tearing down later, for now we focus on assertions and testing.

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

## Practice
Let's practice by
* splitting the test_mul code into two tests .. test_mul and test_conjugate
* writing tests for sub, div, and ne

## Quaternion code
``` python
'''
    The Quaternion class represents elements of Hamilton's quanterions.

    Quaternions are like complex numbers a+bi
    but they have two more imaginary values j,k with
    i^2 = j^2 = k^2 = ijk = -1
    and ij=k=-ji, jk=i=-kj, ki=j=-ik
    So they have the form a+bi+cj+dk and are associative and non-commutative.

    author: Tim Hickey and chatGPT
    date: 2/5/2023

    reference: https://mathworld.wolfram.com/Quaternion.html

'''
import math

class Quaternion():
    '''
      a quaternion: r+xi+yj+zk  where i,j,k are imaginary and r,a,b,c are real

      Attributes:
        w: the real part
        x: the coefficient of i
        y: the coefficient of j
        z: the coefficiant of k

    '''

    def __init__(self, w, x, y, z):
        self.w = w
        self.x = x
        self.y = y
        self.z = z

    def __str__(self):
        ''' return a readable version in the form r + ai + bj + ck'''
        return f"{self.w} +{self.x}i+ {self.y}j + {self.z}k"

    def __repr__(self):
        ''' return a version that can be used to reconstruct itself, i.e. the constructor '''
        return f"Quaternion({self.w}, {self.x}, {self.y}, {self.z})"

    def __add__(self, other):
        ''' return the sum of this quaternion with the other  q+p'''
        return Quaternion(self.w + other.w, self.x + other.x, self.y + other.y, self.z + other.z)

    def __sub__(self, other):
        ''' return the difference of this quaternion with the other  q-p '''
        return Quaternion(self.w - other.w, self.x - other.x, self.y - other.y, self.z - other.z)

    def __mul__(self, other):
        ''' return the product of this quaternion with the other  q*p '''
        return Quaternion(self.w * other.w - self.x * other.x - self.y * other.y - self.z * other.z,
                          self.w * other.x + self.x * other.w + self.y * other.z - self.z * other.y,
                          self.w * other.y - self.x * other.z + self.y * other.w + self.z * other.x,
                          self.w * other.z + self.x * other.y - self.y * other.x + self.z * other.w)

    def __div__(self, other):
        ''' return the quotient of this quaternion by the other  q/p'''
        return self * other.inverse()

    def __eq__(self, other):
        ''' test for equality q==r'''
        return self.w == other.w and self.x == other.x and self.y == other.y and self.z == other.z

    def __ne__(self, other):
        ''' test for inequality q!=r '''
        return not self == other

    def __abs__(self):
        ''' return the length of q ... abs(q)'''
        return math.sqrt(self.w * self.w + self.x * self.x + self.y * self.y + self.z * self.z)

    def __neg__(self):
        ''' return the negation of q ... -q '''
        return Quaternion(-self.w, -self.x, -self.y, -self.z)

    def __pos__(self):
        ''' return q for the expression +q'''
        return self

    def __nonzero__(self):
        ''' test that q!=0 '''
        return self.w != 0 or self.x != 0 or self.y != 0 or self.z != 0

    def __lt__(self, other):
        ''' test if self < other '''
        return (self.w, self.x, self.y, self.z) < (other.w, other.x, other.y, other.z)

    def inverse(self):
        ''' returns 1/self, assumes self!= 0 '''
        if self.abs()==0:
            raise Exception('division by zero quaternion')
        return Quaternion(self.w, -self.x, -self.y, -self.z) / abs(self)

    def conjugate(self):
        ''' (w+ai+bj+ck).conjugate() -> w-ai-bj-ck '''
        return Quaternion(self.w, -self.x, -self.y, -self.z)

    def normalize(self):
        ''' maps self into a unit quaternion, i.e. one of length 1'''
        d = abs(self)
        if d==0:
            raise Exception('can\'t normalize zero quaternion')
        return Quaternion(self.w/d, self.x/d, self.y/d, self.z/d)

    def to_tuple(self):
        '''
          this converts a quaternion to a tuple
        '''
        return (self.w, self.x, self.y, self.z)

    def to_list(self):
        '''
          this converts a quaternion to a list
        '''
        return [self.w, self.x, self.y, self.z]

    def to_dict(self):
        '''
          this converts a quaternion to a dict
        '''
        return {'w': self.w, 'x': self.x, 'y': self.y, 'z': self.z}


if __name__=='__main__':
    q=Quaternion(0,1,1,1).normalize()
    print("\n\nhere is a purely imaginary quaternion q of length 1, these always square to -1")
    print('q =',q)
    print('q**2 = ',q*q)  # here we show how to multiply quaternions with *
    print('is q < q*q?',q<q*q) # here we test if one quaternion is less than another, component-wise
   
```
