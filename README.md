#Building Classes

[First, read through this blog post](https://blog.upperlinecode.com/object-oriented-classes-like-baking-cookies-6dc1df534ac3#.h6vmh04q8). You may not understand everything yet, but try to get a sense for what a class is and how it is used.		

After reading, sit with a partner and describe what a class does, and come up with three other examples of things you might want to use a class for.

Let's come up with a new class together.		
Cell phones, while all having unique properties, are typically built in a similar fashion. Let's create a cell phone class.

	class CellPhone
	
	end

That's it! We've created a class. Now we can make as many new cell phones as we'd like - thought they wouldn't be too interesting to work with. What does every cell phone need to have? Let's create these cell phones with some starting traits - a provider, a brand, and a color.

	class CellPhone
	
		def initialize(provider, brand, color)
		end
		
	end
	
This initialize method tells the class what to create the cell phone with from the start. This way, every single cell phone that we make with this class will have these three traits.		
Now for these traits to be useful, we need to make them accessible to the entire class. This is where scope of variables becomes an issue.

	class CellPhone
		
		def initialize(provider, brand, color)
			provider = provider
			brand = brand
			color = color
		end 
		
	end
	
If we use regular variables here to define these inputs, then only the initialize method will know about them. We won't be able to use these variables anywhere else. Ruby has a solution to this problem - it's called the instance variable. 

	class CellPhone
		
		def initialize(provider, brand, color)
			@provider = provider
			@brand = brand
			@color = color
		end 
		
	end

By putting an @ before the variables, we have now made them accessible to the entire class. So - 

	class CellPhone
		
		def initialize(provider, brand, color)
			@provider = provider
			@brand = brand
			@color = color
		end 
		
		def provider
			@provider
		end
		
	end
	
The method `provider` knows what the provider is for this cell phone. It can access that instance variable.		
This method that we just created is called an "accessor method". It allows us to call that variable later in order to see what the provider is. We can create accessor methods for each of our traits.

	class CellPhone
		
		def initialize(provider, brand, color)
			@provider = provider
			@brand = brand
			@color = color
		end 
		
		def provider
			@provider
		end
		
		def brand
			@brand
		end
		
		def color
			@color
		end
		
	end	
	
Great! Now we have access to all of our traits. So if I want to create a new cell phone now, I would call our class and add in our traits.

	my_cellphone = CellPhone.new("verizon", "iphone", "black")

`my_cellphone` is now a new `CellPhone` with the traits provided! I can call these traits using our accessor methods.

	my_cellphone.provider
		>> "verizon"
	
	my_cellphone.brand
		>> "iphone"
	
	my_cellphone.color
		>> "black"

Awesome, we have a pretty well rounded class. But what happens if I want to change one of these traits after creating the class? I don't have any way to do that right now. But - just as we have accessor methods, we can also create setter methods. Maybe, in this case, I want to be able to change the color of my phone. Let's look back at our class.

	class CellPhone
		
		def initialize(provider, brand, color)
			@provider = provider
			@brand = brand
			@color = color
		end 
		
		def provider
			@provider
		end
		
		def brand
			@brand
		end
		
		def color
			@color
		end
		
		def color=(color)
			@color = color
		end
			
		
	end	

We've now created a method that allows us to set the color to a new value. Now I can say:

	puts my_cellphone.color
		>> "black"
	
	my_cellphone.color = "purple"
	puts my_cellphone.color 
		>>"purple"

My setter method let's me change the color!

I can also create instance methods within my class. These methods let me use any of the variables to print out new data. For example, let's say we wanted our cell phone to be able to print out all of it's info.

	class CellPhone
		
		def initialize(provider, brand, color)
			@provider = provider
			@brand = brand
			@color = color
		end 
		
		def provider
			@provider
		end
		
		def brand
			@brand
		end
		
		def color
			@color
		end
		
		def color=(color)
			@color = color
		end
			
		def info
			print "I am a #{@color} #{@brand} with #{@provider}."
		end
		
	end	
	
This method is actually taking our instance variables and using them - it creates a new functionality for our cell phone.

	my_cellphone.info
		>> "I am a purple iphone with verizon"

Awesome!

I bet your programmer senses are tingling though - isn't this a lot of writing to do something pretty basic? Why do we need to create so many methods just to be able to access simple attributes? There's a solution for this!		


##attr_reader
If we have variables within a class that we want to be able to access, we don't have to write out several methods to do so. Instead, we can call them using the `attr_reader` property. Let's take a look at our class again.

	class CellPhone
	
		def initialize(provider, brand, color)
			@provider = provider
			@brand = brand
			@color = color
		end 
		
		def provider
			@provider
		end
		
		def brand
			@brand
		end
		
		def color
			@color
		end
		
		def color=(color)
			@color = color
		end
			
		def info
			print "I am a #{@color} #{@brand} with #{@provider}."
		end
		
	end	

Those first three methods are only there to help us call these attributes. Let's use the att_reader instead.

	class CellPhone
			attr_reader :provider, :brand, :color
	
		def initialize(provider, brand, color)
			@provider = provider
			@brand = brand
			@color = color
		end 
		
		def color=(color)
			@color = color
		end
			
		def info
			print "I am a #{@color} #{@brand} with #{@provider}."
		end
		
	end	

Woah! With just one line of code, we got rid of three whole methods! This class works exactly the same as it did before, now it's just a lot simpler.		
If we can do this with accessor methods, can we do it with setter methods too? Absolutely!

##attr_writer
Anything that we want to be able to reset for our class can be written as an `attr_writer`. In this case, `color` is the only attribute we have a setter method for. Let's rewrite it using `attr_writer`.


	class CellPhone
			attr_reader :provider, :brand, :color
			attr_writer :color
	
		def initialize(provider, brand, color)
			@provider = provider
			@brand = brand
			@color = color
		end 
			
		def info
			print "I am a #{@color} #{@brand} with #{@provider}."
		end
		
	end

So much prettier! But we can get even nicer here - what if we want to be able to access and set the same attribute? We can combine the two using `attr_accessor`. So now:


	class CellPhone
			attr_accessor :color
			attr_reader :provider, :brand
	
		def initialize(provider, brand, color)
			@provider = provider
			@brand = brand
			@color = color
		end 
		
			
		def info
			print "I am a #{@color} #{@brand} with #{@provider}."
		end
		
	end	


This class can now both access and set the `color` attribute, and access the `provider` and `brand` as well. 
