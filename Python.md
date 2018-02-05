# Python
## Basics

### Variables 
* Created at dynamic time. 
* The size is figured when the variable is being created. for example, in a=a+b. the a is created again thus it can hold as much data as needed at runtime. 
* the data type of a is associated at Dynamically 

Translation an array of byte to make sense of binary data is needed whenever an external data is being read. you would have to write a encode and a decode method. Array of bytes gets created when making use of encode. 

### Sequences
* String : array of UNICode characters. 
* Dictionary : collection os key value pairs 
* Sets : collection of unique values. used to test membership of values. 
* List can hold any type of datatypes. Same list can hold anytype of object. lists should be used when we do want the data to be changing a lot of time. 
* Tuple : immutable list. They are more memory efficient in term of memory so should be used by default. 

#### Slices
aList [1:3] (insert from 1 until 3 not including 3)
    
    aList =['1', '2', '3','4']
    aList[1:3] = ['2.1','2.2']
    aList[1:1] = ['1.2','1.3'] //inserts after 1
    aList += ['5'] //inserts after 1
    aList[1:4] = [] // deletes 2 and 3
    aList[-1] = '4' // Returns the last element

#### Dictionaries
* Key value pairs 
* Almost like a Map in java

#### Example 
Python doesnt have any behaviour for any operator. For every object internally the operators are defined. for example 

	a>b => a.__gt__(b)
	
anything can be made *iterable* by created a next method for the collection. the next gives the next object and when cant(done with iterating basically) throws an exception 
Python can do Operator loading : 

		def plus(a,b)
			return a+b
		def minus(a,b)
			return a-b
		def default(a,b)
			return 'not supported'
		ops ={'+': plus, '-': minus}
		result = ops[op](num1,num2)
		result = ops.get(op, default)(num1,num2)// calls default method when oop not there
		result = ops.get(op, lambda a,b : 'not supported' )(num1, num2)

## Standard Program Layout 
Action statments like : def, import etc. they are called sequentially. 
All these result into stuff being inserted into the symbol table. basically they result in whats part of the symbol table so as to determine the flow of the program. 

    import sys
    other imports  (standard library, standard non-library, local)
    constants (AKA global variables)
    main function
    def main(args):
        pass
    other functions
    def function1():
        pass
    if __name__ == '__main__':
        main(sys.argv[1:])

**Type function** : returns the datatype . 

**isinstance** to check the datatype of variables being paseed to function

Anything with a py extension can be imported. However modules which have name **-** in them they cannot be imported. Interpreteor which trying to import it will give an error because the interpretor will try to delete the second half of the name from the first half. 

** _ builtin _** There are many fucntions which are by default added into a object which would make the object sortable, etc. 

####Print
in Python 2 Print was a statement. In Python 3 Print is now a Function. 

    print(sep=' ', end'\n')
	fle_object = open(flename, mode="r")
	user_data = input(prompt)

* *sep*: the seperator what we want. 
* *end* : what the end would look like. 
* *file* : print's output can be written into the File directly into a file. 

Now the Files can be opened as a text file or a binary File . before Python didnt care about it. 
Binary file is just like a stream of bytes. With mode being set as text implies that Now it will decode automatically as a UNICOde character. 

## OS
Same commands in python can be used to make a dir be it a Windows machine or a Unix machine. Os is alias for posix in mac and so forth 

*sys* : things related to the Interpretor python is running on. 
contains variable **path**, which tells the directories where the python import command is going to look for importing a module. 
*sys.platform* tells the platform like darwin, posix, etc 

**os.popen* doesnt work that well with Windows. Basically it goes off and creates another subprocess and runs the command u gave it, will return a File object which can be used to read the output . It doesn get killed when the parent calling python process dies. 

*subprocess* Better use popen using this module, because there you get a better control over the program that we created. example : you can tell it shell = false this would mean that the program would be just executed at kernel level without having it create a shell, which then in turns runs the process. 

#### Directories 
path.os.exists(), path.os.dirname(), path.os.basename, and path.os.join().
has functions for Everything to determine mount point, File name, etc. 

os.walk and os.path.walk
os.walk : returns a 3 item tuple. dir, subdirs in the dir, files in the dir
however os.path.walk takes in 3 methods. : things to do on the dir, things to do on the subdir, and things to do on the files in the dir. Each method takes in 1 argument. 



####File : 

	with open('knights.txt') as f:
		data = f.readlines()
creating a dictionary 
    
    lbl = ('name','title')
    recs=[]
    for rec in data :
        //chop off new line and split based of delimiter
        rec = rec[:1].split(':') 
        rec= dict(zip(lbl,rec)) //matches the lables with the list vales : creates a list of key value pairs
        recs.append(rec) //puts the key value pair dictionary in the list 
		
cool way of doing this in one line of code. 

    lbl = ('name','title')
    recs=[]
    for rec in data :
        rec = rec[:1].split(':') 
        //creates a dictionary from item0 and 1 pair
        d = {item[0]: item[1] for item in zip(lbl,rec)}
        // even shorter
        recs.append({item[0]: item[1] for item in zip(lbl,rec[:1].split(':') ) })


#### Function Arguments
* **default param values**, and they must be present in the leftmost positions.  
* **Variable no of params** we can pass in a dictionary too to a python function 

The order is this, normal param then default ones, variable no of positional params(list), variable no of named params(Dict)

	def func(aVar, bVar='aaa', *cvar, **dVar)

This was you can upack the parameters at runtime. 

	def someFunc(aVar, bVar):
    	print('aVar={0} bVar = {1}'.format(aVar, bVar))
	data = {'aVar':10, 'bVar':20}
	someFunc(**data)    

Data inside the dictionary is not ordered internally. the data is stored in form of red black tree. 

#### Lambdas
they can be just one expression long in Python

    function-name = lambda param-list: expr

List comprehension is faster than for loop because it gets optimized. However, it creates a seperate result list . 

	nums = range(10)
	squares = [num**2 for num in nums if num%2 ]
Changed it to be a generator expression, so now the its iterating the list and getting just one element from the list at a time. Its almost as fast as list comrehension. This is more like an iterator rather than a processing on the stream. 

	squares = (num**2 for num in nums if num%2 )
	
**range** : list of integers in memory from 1-value provided. 
xrange in python 2. no more xrange in python3. 
range behaves like a generator, and would expand the elements only when needed. 

    suits = { 'H','D','C','S'}
    ranks = {'A', 2, 3, 4, 5, 6, 7, 8, 9, 10, 'J','Q','K'}
    cards = [(rank, suits) for suit in suits for rank in ranks ]
    print (cards)
        

Random no generator within Python should be avioded. 

#### Generators
**Yeild** it would get the record and then give the control back to the calling method and the method stays on the stack and just goes to sleep untill the control method calls it again.  

    def read_data():
        with open('file.txt') as f:
            for rec in f:
            yeild rec[:-1]//delete new line char
    def data in read_data:
        print(data)
    
its like a return, so in case I cam not calling it from a for loop, this function would just stay on the call stack. 

A generator is like a normal function, but instead of a **return** statement, it has a **yield** statement. Each time the yield statement is reached, it provides the next value in the sequence. When there are no more values, the function calls return, and the loop stops. A generator function maintains state between calls, unlike a normal function.

#### Dictionary Comrehensions 

    file_sizes = { f:os.path.getsize(DIR + f) for f in files }

#### String IO
Python just appends adjacent strings. Should not be comma seperated. 
*StringIO* object is initialized with a string, but acts like a normal fle object such as you get from the **open()** function. You can iterate through its lines, or call **read()** or **readlines()** on it.

Can be formatted very any manner

	result = 'a = |{0:7.3.f}|'.format(a)
Format can also take in a list as paramter. They can be access using index directly *{0}* or like element and then its index *0[1]*

Format is also able to understand **named placeholders**

	result= 'foo ={foo} bar ={bar}'.format(bar=10, foo=10)
    result= 'foo ={foo} bar ={bar}'.format(**aDict)
    print(result)
Even if the Dictionary has more elements than the no of keys provided in the fomat specifier, it would still work, however it isnt the same with a list 


## ObjectOriented	
No Polymophism in Python 

Class in python are a namespace which can create objects. 

    class Person:
    	def foo(self):
			print('Inside Person::foo')

	print(dir(Person))// prints namespace of the class

when u declare a class in python it gets allocated some space. and in this space the methods are placed. 

A variable comes into existance only when u assign it a vaule. 

    class Person:
    	def foo(self):
			print('firstname : {0}'.format(self.firstName))
	p = Person()
	p.firstName = 'John'
	p2=Person()
	p.foo
	p2.foo
	
Even though the class doesnt know about firstName it can be accessed in the print line. however *p2.foo* would give an error, because this object doesnt even have the variable firstName. 

    class Person:
		def __init__(self, firstName, secondName):
			self.firstName = firstname
			self.secondName = secondName
    	def foo(self):
			print('firstname : 
	p=Person()
	p2=Person('Sarah','Doe')

**Constructor** it is just a initilizer. p would get created with the values for the variables as empty. 

**Destructor** are also present. Python garbage collector is petty agressive. as soon as there is no reference to the object the object is deleted. 

    class Person:
		def __del__():
			print('deleting')
	p=Person()
	p=None //makes call to the destructor

When you creates a class Python 3 creates a lot more methods which would help various things. In Python <3 versions one would have to inherit the class from Object to get them to work properly. 


in Python the keyword self is not a keyword, its just the way python uses to reference *this* variable. We can use this name for the same work. 

Methods are present in the class. If u change the reference for one, it changes for all of them. 

Variables by convention can be made private using the _ convention. 

**Name mangaling** if a variable is created using **__** the namespace will show it in a different way, so it doesn't make the variable less visible, just a bit difficult to find. 

    class Person:
		def __init__(self, firstName):
			self.__firstName = firstname
    	def foo(self):
			print('firstname : {0}'.format(self.__firstName)
	p=Person('John)
	p._Person__firstName = 'Snow'
	p.foo()

#### Properties
They are as close as python can come to provide a private attribute. 
They are putting a wrapper around a function. 

    class Person:
		def __init__(self, name):
			self.__name = name
    	@property
    	def name(self):
    		return self.__name
    	@name.setter
    	def name(self, name):
    		self.__name = name
    	@name@deleter
    	def name(self):
    		del self.__name //deletes the variable
    	name = property(get_name, set_name)	
    	def foo(self):
			print('Name : {0}'.format(self.__name)
	p=Person('John)
	p.firstName = 'Snow'
	p.foo()
Getter must be before the setter, because only then it can know which property setter it needs to call. 

To make a variable ReadOnly you do not provide a setter to it. 
Property **deleter** can also be defined. which is to delete a property within the object. If you do want to delete the object, one has to explicitly write the del with the name.. 


Way to do this before 2.5 : 
    
    class Person:
		def __init__(self, name):
			self.__name = name
    	def get_name(self):
    		return self.__name
    	def set__name(self, name):
    		self.__name = name
    	name = property(get_name, set_name)	
    	def foo(self):
			print('Name : {0}'.format(self.__name)

#### Operator overloading


	class Money
		def __init__(self, amt=0.0, curr='USD'):
			self.amt = amt
			self.curr=curr
    	def __str__(self):
    		return '{0:.2f} {1}'.format(self.amt, self.curr)
		def __add__(self,other):
			if not isinstance(other, Money):
				raise TypeError('Can only add Money')
			if (self.curr != other.curr)
				raise TypeError('Can only add Money of same curr')
			return Money(self.amt+other.amt, self.curr)

Method | Function | 
------------ | ------------- |
new | Constructor  for python, used very rarely |
radd | Add with the object on the right
iadd | called with +=


####Inheritance
	class Parent:
		def doParentStuff(self):
			print('Parent')
	class Child(Parent):
		def doChildStuff(self):
			print('Child')
Child object will call parent and then child 
U can manipulate the children symbol table for determining which of the method needs to be called. 

	class Parent:
		def doParentStuff(self):
			print('Parent')
	class Child(Parent):

		//TODO CHECK which version has what 
		def doParentStuff(self):
			super().doParentStuff()// in version 3			super(Child, self).doParentStuff()// in version 3
			//Doesnt prevent u from calling grandparent
			Parent.doParentStuff(self) //Used instead of super
			print('Child:doParentStuff')
		def doChildStuff(self):
			print('Child:doChildStuff')
 

In Python the __init__ methods are not called automatically for the parents. So inside the child class __init__ call the parents class __init__ in the order of inheritance. 
And we would need to pass in the **self** vairable because every member variable is associated with self. 
Also, because there is no concept of private variables in Python, I cannot have same variable in both child and Parent class. Its just same instance which would share the various attributes. 
	
Python is very flexible because of which it allows you to do anything, but downside is you would have to do everything. :P 

#### Multiple Inheritance

	class GrandParent:
		def __init__(self):
			print('GrandParent')
		def doGrandParentStuff(self):
			print('GrandParent:doGrandParentStuff')
	class Parent(GrandParent):
		def __init__(self):
			GrandParent.__init__(self)
			print('Parent')
		
		def doParentStuff(self):
			print('Parent:doParentStuff')
	class Uncle(GrandParent):
		def __init__(self):
			GrandParent.__init__(self)
			print('Uncle')
		def doUncleStuff(self):
			print('Parent:doParentStuff')
	class Child(Parent, Uncle):
		def __init__(self):
			Parent.__init__(self)
			Uncle.__init__(self)
			print('Child')

* Python goes a **left to right Depth first search for resolution**
* Super doesnt work, you would have to use the Class name and then call the method. 
* GrandParent will be called twice and this can only be stopped by manually adding in a parameter which marks the method being called twice, There is no way Python stops it for you. 

### Interfaces 
* NO INTERFACES
* No abstract classes 
* Can be created by creating class and then returning errors if the classes calling them do not implement them
* However these are executed at runtime and not compile time. 
* **abc module** It allows you to create abstract base classes. These classes may not be directly instantiated, and if abstract methods are not overwritten, an exception is raised upon instantiation.


## Modules and packages 
* In python a import creates a seperate namespace with an import statment 
* Call to an imports function is done using : **module.function()** this keeps collisions from happening. 
* .pyc in a semi compiled file which just helps in code being loaded a bit quickly. no affect on running time. 

* in python you can differentiate between a import module and being run as an application **using its name**  
* **PyDocs** which would tell you how to use the module. 

Here are examples : 

	class Module:
		def doStuff(self):
			#
	if __name__ = '__main' : 
		# do things to test itself maybe basically this class is being called as app and not being imported. 
		
Import can be done with a different name 

	import myModule as mm 
	would create mm as an entry into the symbol table 
	
Import can be done into the current namespace using : 

	from myModule import *
	would get the method doStuff into the current namespace

#### Package
* package is a folder containing modules
* Packages may contain packages as well
* Package startup code goes in __init__.py in the top directory of the package. mandatory for 2version
* Packages can be nested 

		__init__ for foo
			import myModule
			
		import foo
		foo.myModule.doStuff
One can import whole set of methods in different modules in the the package into the package itself. 

		__init__ for foo
			from myModule import *

		import foo
		foo.doStuff

#### Name Resolution
Scope of a vairable : 

* local method 
* then the scope of the package where the method was defined. 
* contents of __builtins__ module

		aVar = 1

		def doStuff():
			global aVar //Creates a local copy of the global aVar global in this file
			aVar = 2
		print(aVar)	
		doStuff()
		print(aVar)			

		Output : 1 2


## Metaprogramming
Writting code which modifies existing code at Runtime. 
**Monkey patching** Dynamically add new methods to objects
We can replace the existing method by using the method **setattr** to be a different method at runtime. 
**delattr** deletes an attribute and its corresponding value.

**Insepect module** Gives info about the module 

### Decorator 

	def transactional(old):
		def transMgr(*args, **kw):
			print('starting transaction')
			try:
				result = old(*args, **kw) # this is executing the doStuff method
				print('Committing transaction')
				return result // needed to return the value being returned by the method doStuff
			except:
				print('rolling back transaction')
		return transMsg # returning the method function so that the doStuff points at the trasMgr instance which was used to internally call the doStudd implementation 
			
	@transactional
	def doStuff(a, b, c):
		print('doing bsuinesswork')
		print('params a={0} b={1} c={2}'.format(a, b, c))
		return 'WOW!!!'
	
	print(doStuff(42, 99, 'hello'))

A Cleaner way to do this : 

	class transactional :
		def __init__(self, old): #self and reference to method being decorated
			self._old= old
		delf__call__(self, *args, **kw): # vairable positional args and variable key-word args
			print('Starting transaction')
			try:
				result = self._old(*args, **kw)
				print('Committing transaction')
				return result
			except:
				print('rolling back transaction')
			
	@transactional
	def doStuff(a, b, c):
		print('doing bsuinesswork')
		print('params a={0} b={1} c={2}'.format(a, b, c))
		return 'WOW!!!'
	
	print(doStuff(42, 99, 'hello')) 
In this case the doStuff is now a transactional object. Its not a method for the parent Class. 
And now we are just invoking the object as it was a method and then providing it with certain Values. 


Using this was the initialiazed objetc could be provided properties as well. 


	class transactional :
		def __init__(self, old, lockLevel): 
			self._old= old
			self._lockLevel = lockLevel
		delf__call__(self, *args, **kw): # vairable positional args and variable key-word args
			print('Starting transaction')
			try:
				result = self._old(*args, **kw)
				print('Committing transaction')
				return result
			except:
				print('rolling back transaction')
	
	@transactional(lockLevel=42)
	def doStuff(a, b, c):
		print('doing bsuinesswork')
		print('params a={0} b={1} c={2}'.format(a, b, c))
		return 'WOW!!!'
	
	print(doStuff(42, 99, 'hello')) 

When python makes a call to a method using params of the form : b=99 a=42 etc, then the args are passed in as a Key-word pair

Using wraps we can access the pyDocs of the decorator method. 


	class Worker:
		def doStuff:
			
	w=Worker()
	w.doStuff()
	def foo()
		print('In replacement')
	setattr(Worker, 'doStuff', foo)		
	w.doStuff -> calls foo
	setattr(Worker, 'doOtherStuff', foo)		
	w.doOtherStuff -> calls foo
	Worker.doOtherStuff = foo
	w.doOtherStuff -> calls foo
	w.doOtherStuff = foo
	w.doOtherStuff -> calls foo 
	w1.doOtherStuff -> doesnt call foo.. 
	
Functions are just pointers to some point of code. 

### Databases
* Different way to create connection with every database
* Cursor return list based dat. 
* Dictionary form of output is not returned by every version. 
* Orm framwork by Django similar to hibernate in java
* Different DB's have different way of how the connection is made

		import sqlite3 as db

creating dictionary :

	def row_as_dict(cursor):
    	'''Generate rows as dictionaries'''
    	column_names = [desc[0] for desc in cursor.description]
    	for row in cursor.fetchall():
        	row_dict = dict(list(zip(column_names, row)))
        	yield row_dict


## Networking 
* make use of ParaMiko library for ssh related stuff 
* has stuff to setup reverse proxy 

#### Java Script
* import json Load and dump from any stream. loads and dumps to load/dump onto a string object. 
* Requests.get and put for Web IO
* Auth detials are provided in the conect string itself. 


#### SMTP
* library for Python just allows you to connect with the SMTP server, the formatting of the msg and everything will be the users reponsibility 

#### Binary 
* data manipulation with python can get pretty complex 
* Can be done using **struct** package

