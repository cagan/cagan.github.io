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

    public function swap($newPlan)
    {

    }
}
```


Think about this methods. For example cancel. To cancel we need more than update the users table in database. Maybe we need to fire off an API request. Let's say we use Stripe API to handle our billing processes. So we need some additional functionality to our class. Like we need to find stripe customer and stripe subsription by customer. How can we handle it?
There are three ways to add this behavior. Lets look at them one by one.
The first one is the obvious way. We just add the behavior at the same class and everything.

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

    public function swap($newPlan)
    {

    }

    public function findStripeCustomer()
    {
        // API request
    }

    public function findStripeSubscriptionByCustomer()
    {
        // Another API request.
    }
}
```

Easy! But there are problems with that. If you think about the SOLID principles this violates Single Responsibility, and Open-Closed methodologies. We have to change the class everytime we need to change the behavior of that class. And this is not good thing! Also our class should't do Stripe processes here. Its not his responsibility. These two methods are not related with Subsription. They moslty like related with BillingProvider. 

Let's move on the option two. We can use inheritance. Let's say we have another subsription called BillableSubscription and it will be a subclass of Subsription class. So we can move our two methods related with subscription into the BillableSubscription. It is one of the tecnique that we can reduce code duplication. 
So let's improve the code. 

```php
class Subscription extends BillableSubscription {

    public function create()
    {
        
    }

    public function cancel()
    {
        
    }

    public function invoice()
    {
        
    }

    public function swap($newPlan)
    {

    }

}

class BillableSubscription extends Subscription {

    protected function findStripeCustomer()
    {
        // API request
    }

    protected function findStripeSubscriptionByCustomer()
    {
        // Another API request.
    }
}
```


So our code look much clear than before and easy to understand. 
But there is a problem. These methods aren't related with the subscription. They are related to almost like the Gateway to BillingProvider. And it is contained in that class. We might have methods like this that might be used elsewhere in our system. That causes duplication again. Remember we don't want to duplicate our code. We decided inheritance is not quite right choice here. 

So move on option three. Composition. Remove the extends keyword with the class. Change accessor to public and change class name as "StripeGateway". I think it is a good name to understand its responsibility. Let's see again. 


```php
class Subscription  {
    protected StripeGateway $gateway;

    public function __construct(StripeGateway $gateway)
    {
        $this->gateway = $gateway;
    }

    public function create()
    {
    }

    public function cancel()
    {
        
    }

    public function invoice()
    {
        
    }

    public function swap($newPlan)
    {

    }

}

class StripeGateway {

    public function findStripeCustomer()
    {
        // API request
    }

    public function findStripeSubscriptionByCustomer()
    {
        // Another API request.
    }
}
```



Now we have $gateway pointer to the another class. This is object composition. Lastly let's add GatewayInterface and pass throught as constructor parameter so we can decouple our dependency. 

```php
interface GatewayInterface {
    public function findCustomer();
    public function findSubscriptionByCustomer();
}

class Subscription  {
    protected StripeGateway $gateway;

    public function __construct(GatewayInterface $gateway)
    {
        $this->gateway = $gateway;
    }

    public function create()
    {
    }

    public function cancel()
    {
        $customer = $this->gateway->findCustomer();
    }

    public function invoice()
    {
        
    }

    public function swap($newPlan)
    {
    }

}

class StripeGateway implements GatewayInterface {

    public function findCustomer()
    {
        // API request
    }

    public function findSubscriptionByCustomer()
    {
        // Another API request.
    }
}
```


