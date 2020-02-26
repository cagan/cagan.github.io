---
layout: post
title:  "Design Patterns 1: Template Method Pattern"
author: "Cagan"
comments: false

---

I think it is one of the most useful design pattern. I realized that I've using this pattern very often without knowing it is actually a design pattern. Let's see how [Wikipedia](https://www.wikiwand.com/en/Template_method_pattern) defines what Template Method Pattern is:

> "In software engineering, the template method pattern is a behavioral design pattern that defines the program skeleton of an algorithm in a method, called template method, which defers some steps to subclasses."

When I see an explanation like above my mind blows up... Let's try with our words.

In Object Oriented Programming it is easy to face code duplication in any code that we write. In order to prevent this we can use Template Method Pattern. It is a design pattern that help us to prevent duplication of code within the subclassess which do the similar things. It defines the skeleton of the algorithm in the superclasses override specific steps of the algorithm without changing its structure.

### TL&DR

Imagine we are making a video game. It is a Realtime Strategy Game, and we have 2 races. One of them is Orcs and the other is Humans. Both races have units. And any race that we choice has ability to build structure, collect resources, create workers and produce soldiers.

Lets create our **Orcs** class: 

```php
class Orcs
{
    public function play()
    {
        $this
            ->collectResource()
            ->buildStructure()
            ->createWorker()
            ->produceSoldier();
    }

    public function collectResource()
    {
        var_dump('Gaining minerals.');

        return $this;
    }

    public function buildStructure()
    {
        var_dump('Building barracks...');

        return $this;
    }

    public function createWorker()
    {
        var_dump('Populating workers to collect more mineral.');

        return $this;
    }

    public function produceSoldiers()
    {
        var_dump('New orc soldier being ready. Ready to fight!');

        return $this;
    }
}
```

> *For the Horde!* 

Our **Orcs** ready to fight now. 
Let's do it for the Humans so we can fight and enjoy our game.
There is no `createWorker()` method for now. But lets bear with me, we will change it soon.

```php
class Humans
{
    public function play()
    {
        $this
            ->collectResource()
            ->buildStructure()
            ->createWorker()
            ->produceKnights();
    }

    public function collectResource()
    {
        var_dump('Gaining minerals.');

        return $this;
    }

    public function buildStructure()
    {
        var_dump('Building barracks...');

        return $this;
    }

    public function createWorker()
    {
        var_dump('Populating workers to collect more mineral.');

        return $this;
    }

    public function produceKnights()
    {
        var_dump('New knights being ready. Lets kill some Orgs!');

        return $this;
    }
}
``` 

Our human race is ready to fight as well. So lets look at the both classes. If we take a look at these classes we can see that both classes look so similar except one method. 
However some of the things are not identical exactly. Humans have knights and Orcs have soldiers. `produceKnights()` and `produceSoldiers()`. And we use the same methods in both classes. If we think about for the current code it may not be a problem for the developer team. But if we predict the future and after code grows and become huge developers wanna suicide and it will be chaos.

Let's try to fix it: 
If we think about the similarities between two classes the first thing that comes in my mind is using Inheritance. We will use the abstract class so we can dictate subclasses to implement some methods they should implement. So we can remove the duplication of the code. 


```php
abstract class Races
{
	// Implementation of the general things that all races should do  
}
```

If we think about the duplication lets move these three functions into the **Races** class: `createWorker(), buildStructure(), collectResource()`. 

```php
abstract class Races
{
    public function collectResource()
    {
        var_dump('Gaining minerals.');

        return $this;
    }

    public function buildStructure()
    {
        var_dump('Building barracks...');

        return $this;
    }

    public function createWorker()
    {
        var_dump('Populating workers to collect more mineral.');

        return $this;
    }
}
```


So far so good now. Lets declare our abstract method so the other subclasses can implement that method in their own way. But we need to find a common method name so both classes can implement without any confusion. What about `produceUnit()`? Lets implement too. 

```php
abstract class Races
{

    public function races()
    {
        $this
            ->collectResource()
            ->buildStructure()
            ->createWorker()
            ->produceUnits();
    }

    public function collectResource()
    {
        var_dump('Gaining minerals.');

        return $this;
    }

    public function buildStructure()
    {
        var_dump('Building barracks...');

        return $this;
    }

    public function createWorker()
    {
        var_dump('Populating workers to collect more mineral.');

        return $this;
    }

    abstract public function produceUnits();
}
```

Notice we created `produceUnits()` method as an abstract method. And we changed the method name as `produceUnits()` into the `play()` method. So everyhing is almost ready except implementing the abstract methods in subclasses.

```php
class Humans extends Races
{
    public function produceUnits()
    {
        var_dump('New knights being ready. Lets kill some Orgs!');

        return $this;
    }
}

class Orcs extends Races
{
    public function produceUnits()
    {
        var_dump('New orc soldier being ready. Ready to fight!');

        return $this;
    }
}

```

Because we moved the common methods in parent class we removed the common methods. Because we can already access from the parent method. And only thing that `Orcs` And `Humans` class do is implementing the `produceUnits()` method. And we are free to implement our unique logic for each subclassess.
Lets initialize both classes.

```php
(new Orcs())->play();
(new Humans())->play();
```

```bash
/Users/cagan/Dev/orcs_and_humans/src/Races.php:21:
string(17) "Gaining minerals."
/Users/cagan/Dev/orcs_and_humans/src/Races.php:28:
string(20) "Building barracks..."
/Users/cagan/Dev/orcs_and_humans/src/Races.php:35:
string(43) "Populating workers to collect more mineral."
/Users/cagan/Dev/orcs_and_humans/src/Orcs.php:11:
string(44) "New orc soldier being ready. Ready to fight!"
/Users/cagan/Dev/orcs_and_humans/src/Races.php:21:
string(17) "Gaining minerals."
/Users/cagan/Dev/orcs_and_humans/src/Races.php:28:
string(20) "Building barracks..."
/Users/cagan/Dev/orcs_and_humans/src/Races.php:35:
string(43) "Populating workers to collect more mineral."
/Users/cagan/Dev/orcs_and_humans/src/Humans.php:11:
string(45) "New knights being ready. Lets kill some Orgs!"
```

Now we are ready to fight!