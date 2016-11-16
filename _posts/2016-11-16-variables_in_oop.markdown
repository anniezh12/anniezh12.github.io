---
layout: post
title:  "Variables in OOP"
date:   2016-11-16 20:04:13 +0000
---


In Object Oriented Programming we get to see a lot of  variables ..For example Instance Variable, Class Variable, and Local Variables. 

**  Instance Variable**

Lets discuss them in detail, consider a class named Teachers

```
class Teachers

attr_accessor :name

def initialize(name)
 name
end

def print_name
puts name
end

end
 
```
 now we create an object of this class with our first teacher name 
 
 Muhammad = Teachers.new("Muhammad") # an Instance of class Teachers is being created with a name " Muhammad"

Muhammad.print_name

 what do you expect from th eabove code will it assign name provided by us when creating object of class "Teachers"?
 
 we will get the following error message
 
` NameError: undefined local variable or method `name' for #<Teachers:0x0000000174b3b8>`
 
  It looks like name variable in initialize method has a local scope and is known to initialize method only ... How can we make it accessible to other functions of the class ? 
	For this reason Ruby provide us a very simple method and that is using @ symbol before a variable name and it will make it accessible to all the methods in a class, such variable is known as "Instance Variable"  we can write the above program as follows
	

```
class Teachers

attr_accessor :name

def initialize(name)
 @name = name
end

def print_name
puts @name
end

end
```

now we create an instance of class Teachers

Hanna = Teachers.new("Hanna") # an Instance of class Teachers is being created with a name " Hanna Khan"

Hanna.print_name

=> Hanna Khan #output

our instance variable provided us ability to access name which we supplied at the time of object creation

**class Variable**

In the above example we used an instance variable and call function print_name on that instance. Can we keep track of all the teachers being created using some instance variable for example @names ?
The answer is yes through arrays for example  we write the above program as follows

```
class Teachers

attr_accessor :name, :names

@names = [] 

def initialize(name)
     @name = name		 
		 @names << name
end

def print_name
     puts @name
end

def print_names
     @names.each{|teacher_name| puts teacher_name}
end

end
```

Now we create three teacher instances

```
aniqa = Teachers.new("Aniqa")
saad = Teachers.new("Saad")
umar = Teachers.new("Umar")
```

and call function print_names

`aniqa.print_names`

our expected result should be

Aniqa  
Saad  
Umar  

But we get an undefined method error .. why is that? 
The answer is that we defined names[] as an instance variable so all it can do is to work with an instance and being unaware of other instances being created. So any push method (<< ) wont work on it .

How can we fix this problem? And the answer is very easy just add another @ symbol before @names (i.e @@names)
and it will make it a class array and we will be able to push information of each new instance to it such variable is known as a Class Variable. We can write the above program as follows 


what will be the Output?

```
class Teachers

attr_accessor :name, :names

@@names = [] 

def initialize(name)
     @name = name		 
		 @@names << name
end

def print_name
     puts @name
end

def print_names
     @@names.each{|teacher_name| puts teacher_name}
end

end
```

Now we create three teacher instances

```
aniqa = Teachers.new("Aniqa")
saad = Teachers.new("Saad")
umar = Teachers.new("Umar")
```

and call function print_names

`aniqa.print_names`

An we get our desired result as follows 

Aniqa    
Saad     
Umar    

**Local Variable**

It is simple variable which we can use in a class method and it stays there and has a local scope in th emethod
for example in the above example in the print_names method of Class Teachers

```
def print_names
     @@names.each{|teacher_name| puts teacher_name}
end
```

teacher_name is a local variable and has a local scope in the each method.



 
