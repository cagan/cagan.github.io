---
layout: post
title:  "What is IoC And How IoC Container Works"
author: "Cagan"
comments: false

---
When we work with frameworks, many working things can be explained by frameworks instead of the developer. That's why some of the developers (Especially core developers) don't like framewokrs and the programmers that use frameworks. Anyway its another topic. I would like to explain IoC containers. 


IoC container is a principle that can be used for manage dependencies in your project. It manages dependencies for us. So we don't need to manage dependecy injections in our application. IoC handles for us. When we create instance, In traditional procedural programming, system reads the code from the initial point so you should have the control in any part of the system. But when we use frameworks, they take the control from us. They run initilization code, some of the core things like preparing initial resources, loading service providers, initializating middlewares, and creating our requests. When it's time to run our code that we write framework calls our code block and framework takes the control again. It is called "Inversion of Control".Doing core things, running our code, then taking control again and passes to frameworks code again. Any code except ours is handles by the frameworks itself. For example if we develop new Chat app, frameworks like Spring, Laravel, Django will take us the control and do core things for us. So we can just focus on our app. When we run our framework app, it prepares initial resources for us.For example App containers will load service providers that we use for our app. Then our middlewares becomes ready. After that it prepares routes to listen our requests. So many of these processes handle by frameworks. Inversion of Control is a software principle. It provide us to manage of the any instances. It uses DI(Dependency Injection) technique to manage any instance that we use in runtime. It uses DI(Dependency Injection) technique to manage any instance that we create in runtime. 

This post will be updated with examples of Laravel IoC soon.
  
