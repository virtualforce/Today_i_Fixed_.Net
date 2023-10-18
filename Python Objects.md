# Python Objects: Mutable and Immutable

<i><b>Everything in Python is an object.<b></i>. This is the most common phrase you read when you enter the world of Python. All data types and functions that we use in Python are actually the objects of some bigger class. So when we initialize a variable (int, string or list etc.), we are actually creating an object of that class (int, string, list or whatever we initialize). Same is the case when we call a built-in function. This can be viewed as:

variable_name =<i> object </i>

So if all these things are object, this means there must be some attributes associated with Python objects. <i><b>Identity (id)</b></i>, <i><b>type</b></i> and <i><b>value</b></i>, these 3 main attributes are associated with each python object. Understanding of these attributes is necessary to further move to concept of Mutable and Immutable.

## Identity

Identity is the address of object in memory and is represented by a numerical value. Every object in Python has distinct location in memory (remember this line). <b>id()</b> is used to check the address of an oject in our program. To check if two objects in memory have same address, <b>is</b> operator is used. Let's try this.


```python
# intializing 3 variables
alpha = 911
beta = 'Python'
charlie = alpha

#printing the id of variables
print('OUTPUT:')
print('id of alpha: ', id(alpha))
print('id of beta: ', id(beta))
print('id of cahrlie: ', id(charlie))
```

    OUTPUT:
    id of alpha:  2178192041360
    id of beta:  2178152141128
    id of cahrlie:  2178192041360
    

Here we can see that id of aplha and charlie are same, means both were referencing the same address in memory, which contains a value <i>911</i>. We can test this too using <b>is</b> operator.


```python
#using 'is' operator
alpha is charlie
```




    True




```python
#using 'is operator'
alpha is beta
```




    False



## Type

Type, as name suggests, tells us what is the data type of variale that we have initialized. When we intialize a object variable, Python itself detects from which class it belongs. <b><i>Type of object determines what operations can be performed on that object.</i></b> <br>
<b>type()</b> is used in Python to check the <i>type</i> of variables in Python. Let's check the our variables we initiliazed above.


```python
# checking type of variables. type() tell from which class our variable belongs
print('OUTPUT:')
print('Type of alpha: ', type(alpha))
print('Type of beta: ', type(beta))
print('Type of charlie: ', type(charlie))
```

    OUTPUT:
    Type of alpha:  <class 'int'>
    Type of beta:  <class 'str'>
    Type of charlie:  <class 'int'>
    

Look Python detected itself the type of variables. Output tells alpha and charlie belong to class of int/integer whilel beta belongs to str/string class.

## Value

Value is what value we assign to our object. Like we assigned a value <i>911</i> to variable <i>alpha</i> and <i>charlie</i>, and assigned <i>Python</i> to variable <i>beta</i> (let Python take the headache of their types).

Now we have understood the concept of id, type and value, we are ready to understand the concept of <b>Mutable</b> and <b>Immutable</b> objects.

# Mutable and Immutable Objects

As we are learning Python, we should be clear that all the objects in Python are either <b>Mutable</b> or <b>Immutable</b>. And they are pretty simple to understand.

<b><h3>Mutable</h3></b>: Objects whose <b>value can be changed</b>.
<b><h3>Immutable</h3></b>: Objects whose <b>value <b>can NOT be changed</b> once they are created.

<img src="images/Mutable and Immutable types.png" alt="Python Data Objects" height="600" width="600"></img>

<br>Let's understand what these definitions mean. We'll use the three valriables, alpha beta and charlie, defined earlier. <br><br>
First we will se how <b>immuttable</b> obejcts work. As alpha and charlie both are <i>int</i> and int is <i>immutable<i> type. We can not change the value of an immutable object.


```python
#print value and id of alpha
print(alpha)
print('id of alpha: ', id(alpha))
```

    911
    id of alpha:  2178192041360
    

Now let's change value.


```python
#changing value of alpha
alpha = alpha + 5
print(alpha)
```

    916
    

Well Congratulations!! You just changed the value of an immutable object. <br> <br>
But WAIT!!


```python
#print id of alpha
print('id: ', id(alpha))
```

    id:  2178192041008
    

Look id(memory address) of alpha has been changed. So, you didn't changed the value of alpha actually. What you did is that you created a new int object whose value is 916 and it's named <i>alpha</i>. <br><br>
Also we will see value of <i>charlie</i> because charlie and alpha were same as we did charlie = alpha.


```python
#printing value and id of charlie
print('value: ', charlie)
print('id: ', id(charlie))
```

    value:  911
    id:  2178192041360
    

We can see value and id of charlie is not updated and it retained it's attributes. <br><br>
That is how <b>immutable<b> data objects work.

Now we will examine working of <b>mutable</b> objects. Lets first define a mutable object, list.


```python
#defining a list which is mutable
myList = [1,2,'Python',3]
myList_2 = myList
```


```python
#printing value and id of myList
print('myList: ', myList)
print('id myList', id(myList))
print('myList_2: ', myList_2)
print('id myList_2 ', id(myList_2))
```

    myList:  [1, 2, 'Python', 3]
    id myList 2178192129352
    myList_2:  [1, 2, 'Python', 3]
    id myList_2  2178192129352
    

Now as list is mutable, we can change/update it's value inplace(on the same memory address) using Python's built-in function(s).


```python
myList.append('awesome')
```


```python
#printing value and id of myList
print('value: ', myList)
print('id ', id(myList))
```

    value:  [1, 2, 'Python', 3, 'awesome']
    id  2178192129352
    

Value of myList is updated while retaining the place in memory. And if we see value of myList_2, it has been updated too, which didn't happen in case of <b>immutable</b> objects.


```python
#printing value and id of myList_2
print('value: ', myList_2)
print('id ', id(myList_2))
```

    value:  [1, 2, 'Python', 3, 'awesome']
    id  2178192129352
    

Now lets see an case of <b>immutable</b> data objects.


```python
#defining a tuple
myTuple = ('Python', [1,2,3])
print(myTuple)
print('id:',id(myTuple))
```

    ('Python', [1, 2, 3])
    id: 2178190987336
    

In myTuple, we have two values, one is <i>string</i> which is immutable and other is a <i>list</i> which is mutable. We have contained an immutable and a mutable object within an immutable object.<br><br>
As we can change the value of a mutable object, we will do it and see what happens.


```python
# taking value of list from tuple
t_list = myTuple[1]
print(t_list)
```

    [1, 2, 3]
    


```python
t_list.append('awesome')
print(t_list)
```

    [1, 2, 3, 'awesome']
    


```python
#printing value and id of myTuple
print(myTuple)
print('id: ', id(myTuple))
```

    ('Python', [1, 2, 3, 'awesome'])
    id:  2178190987336
    

As you can see above as we updated the value of list, it also got updated inside the tuple. It doesn't mean tuple is mutable. Tuple is a container, an object which can contain other data objects and itself is an object. What we did is that we just updated the values of a mutable object (which we can surely do!) inside a tuple. If we want to change the value of tuple, for example, if we want another instead of a list on second index of tuple, Python will not let us do this because tuple is <b>immutable</b>. We can check this as well.


```python
myTuple[1] = 5
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-18-e896a0a9affc> in <module>()
    ----> 1 myTuple[1] = 5
    

    TypeError: 'tuple' object does not support item assignment


So we see, trying to change an immutable object gives us an error.

At the last, there is another thing needed to be clear about Python object. As I said earlier, every object in Python has distinct location in memory. But in some cases for <b>immutable</b> objects, we will see this statement is not ture.


```python
# defining two variables
a = 5
b = 5

a is b 
```




    True



Output <i>True</i> suggests that <i>a</i> and <i>b</i> have same memory address while both of them are seaparate varibales. This is because of <b>caching</b> in Python. Python caches objects with small values (-5 to 256) and variables with same values are assigned to that object. Like in our example, as <i>a</i> and <i>b</i> have same value, that is why both are referencing to same object. Now if we increase the values of both variables, greater than 256, result will change.


```python
# chaning values of a and b
a = 257
b = 257

a is b 
```




    False



Same is the case for string object. String variables with same value upto 20-25 characters are referenced to same address.

Remember that this <b>caching concept is for immutable objects.</b>

This is all from this tutorial. Hope you enjoyed and understood the tutorial. Kindly let me know about your suggestions at burhan.naeem2@gmail.com .
