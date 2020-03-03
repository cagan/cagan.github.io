---
  1 layout: post
  2 title: "Composition Over Inheritance"
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


There is an alternative solution called **Composition**. Inheritance represents "is a" relationship between classes. A car "is a" vehicle, A person "is a" mammal. But in composition there is a "has a" relationship. For example: A car "has an" engine, a person "has a" name.

So what is composition?
> Composition is the tecqinue where you combine your types (mostly classes) into one peace to create more complex object.


Object composition is when one class has a pointer to another class. When we say pointer, we refer some property on that object. That means we can point it and use its any functionality that we need from another object. With composition, it's easy to change behavior on the fly with Dependency Injection. 

### Example

Let's say we have Subsription class. Let's think about what sort of behavior might Subsription have. You can create one, you can cancel your subsribtion, maybe invoice subscription, you can swap it out for a new plan. (Montly to yearly maybe?).  
So the initial class will look like this: 

```php
class Subscription {

    public function create()
    {
        
    }

    public function cancel()
    {
        
    }

    public function invoice()
    {
        
    }

    public function swap()
    {

    }
}
```


Think about this methods. For example cancel. To cancel we need more than update the users table in database. Maybe we need to fire off an API request. Let's say we use Stripe API to handle our billing processes. So we need some additional functionality to our class. Like we need to find stripe customer and stripe subsription by customer. How can we handle it?
There are three ways to add this behavior. Lets look at them one by one.
1 is the most worst way. Its simplty add new functions to our class directly. 

