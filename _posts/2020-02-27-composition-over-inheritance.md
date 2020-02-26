---
  1 layout: post
  2 title: "Composition Over Interface"
  3 author: "Cagan"
  4 ---
  
  
Inheritance is probably the most obvious and easy way of reusing code between classes. For example you have two classes with the same code. You can create a common base class for these two and move the similar code into it. So easy!

Unfortunately, inheritance comes with caveats that often become apparent only after your program already has tons of classes and changing anything is pretty hard. When our program grows it becomes hard to handle our dependent objects betweeen them. For example you have `fly()` method in your **Animal** base class and you want to extend in your another animal classes. Animal class is abstract class and we have to implement all of these methods into the sublasses in order to obey the rule what Inheritance dictates for us. So if we have `Dog` class we have to implement fly() method which is not inappropriate. Because dogs can not fly. So this is the first program with inheritance that comes up in my mind when i think about the cons.

Let's look at the problems with inheritance.

- **A subclass can't reduce the interface of the superclass.** You have to implement all abstract methods of the parent class even if you won't be using them.
- **When overriding methods you need to make sure that the new behavior is compatible with the base one.**
 It's important because objects of the subclasses may be passed to any code that expects objects of the superclass and you don't want that code to break.
- **Inheritance breaks encapsulation of the superclass ** becuase the internal details of the parent class become available to the subclass. There might be an opposite sitation where a programmer makes a superclass aware of some details of subclasses.
- **Subclasses are tightly coupled to superclasses.** I think it is one of the main program with inheritance. Because all we want to do is make extentiable and large scale applications. That's why we use object oriented programming most of the time. But how we can develop large scale apps with tightly coupled structures? So any change in a superclass may break the functionality of subclasses.

- **Trying to reuse code through inheritance can lead to creating parallel inheritance hierarchies.** Inheritance usually takes place in a single dimension.(Each method or property can be used from the parent class to the child class.)But whenever there are two or more dimensions, you have to create lots of class combinations, bloating the class hierarchy to a ridiculous size.


There is an alternative solution called **Composition**. Inheritance represents "is a" relationship between classes. (Bird is an Animal). But in composition there is a "has a" relationship (Room has table). 

### Example








