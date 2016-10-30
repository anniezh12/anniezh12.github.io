---
layout: post
title:  "OOP and Life"
date:   2016-10-30 01:40:20 +0000
---



Object Oriented Programming(OOP) is little hard to understand but when you take an object as a human, class as the Human Race, methods as functions a human can perfom and data as its characteristics it becomes very easy . 
Each person is born and belongs to a country and has specific attributes like different languages and they celebrate different occasions. Similarly an object is an instance of a class that can exhibit different attributes. Moreover they can perform different tasks based on the class they belong to. 

Lets consider a class Person (the class name should always start with a capital letter, otherwise it will give a syntax error)

class Person
#code
end

We created a class using keyword "class" following the name we want to give to our class, in this case "Person", we can then create an object of this class using keyword "new " as follows:

Qasim = Person.new

Hunniya = Person.new

We have created two new objects of the class Person, "Qasim " and "Hunniya", both will have totally different storage spaces in memory and will not share their data or memory space with each other. That is the most appealing characteristic of OOP.

lets add some functionality to this class by defining methods "Name", "Gender" and "Date_of_Birth" which will actually assigns name, gender of each person born and also set their date of birth. And finally a function which will display all the info about that person/object

 class Person
 
    def Name = (given_name)

             @name = given_name

      end

      def Gender = (gen)

             @gender = gen

      end

     def Date_of_Birth = (supplied_dob)

            @dob = supplied_dob

      end
			
			def Display_info
			
			puts "Hi #{@name}! Your parents had a #{@gender} child on #{@dob} "

       end
			 
	end

now we can repeat the same steps as we defined earlier to create an object

Qasim = Person.new

so we have an object  "Qasim" which can call all the functions defined in the class it belongs to (using dot "." notation) 

Qasim.Name = "Qasim Hassan"

Qasim.Gender = "Male"

Qasim.Date_of_Birth ="03/30/2016"

now we have called three functions and provided them the information about Qasim we can get this information printed calling the final method as

Qasim.Display_info

and we will get the following result 

Hi Qasim Hassan! Your parents had a Male child on 03/30/2016

In the above code I have used @ with variable names and these are known as "INSTANCE VARIABLES"...
Instance variables are available across methods for any particular instance or object. That means that instance variables change from object to object. 

 Ruby is a pure object-oriented language and everything appears to Ruby as an object. Every value in Ruby is an object, like strings, numbers and even true and false. Even a class itself is an object that is an instance of the Class class. For example if we declare a variable name 
 
 name = "Abiya"
	
Now the variable "name" belongs to a built in class string, and hence all the methods defined in that class can be applied to this variable. e.g
	
	name.length 
	
	=> 5
	
	name[0]
	
	=>A
	
	name.split("")
	
	=>["A","b","i","y","a"]
	
So we can conclude that Classes are the blueprints that define the behavior and information our objects will contain. They let us manufacture and instantiate new instances.
