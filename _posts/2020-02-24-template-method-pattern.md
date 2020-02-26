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

Imagine we are making a video game. It is a Realtime Strategy Game, and we have 2 races. One of them is Orcs and the other is Humans. Both races can attack in various ways. And any race that we choice has ability to build structure, collect resources, send scouts. 

```php
class Orcs
{
    public function __construct()
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

    public function produceSoldier()
    {
        var_dump('New orc soldier being ready. Ready to fight!');

        return $this;
    }
}
```



However some of the things are not identical exactly. One of the race uses horses and the other one is uses jackals as a mount.

So lets show this scenario using some PHP codes.

