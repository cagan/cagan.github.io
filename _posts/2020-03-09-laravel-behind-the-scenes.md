---
layout: post
title:  "How Laravel Works Behind the Scenes"
author: "Cagan"
comments: false

---

Everything starts with `index.php` as many web applications. Once we open `example.com` our out-of-the-box-configured webserver(Nginx, Apache, ...) points to the `index.php` file located in index.php. This is the initial point that comes up for each request client send. So let's look at the `index.php` file more closely.

![index.php](https://user-images.githubusercontent.com/12012983/76318576-f75c9600-62ee-11ea-8b4b-173df294640b.png)

This first time essentialy starts up the timer, so you can time how long it takes to boot up teh framework, etc. But its not using anywhere in Laravel.


![autoload](https://user-images.githubusercontent.com/12012983/76319059-b1540200-62ef-11ea-9cbb-1186343118f4.png)

This line loads the composer autoloader file. Every PHP class is loaded in your project can be used without writing `require ExampleClass.php`

![app instancer](https://user-images.githubusercontent.com/12012983/76319364-29222c80-62f0-11ea-900f-5153fd864424.png)

This is where everything starts to get more interesting. `"app"` instance has been created here. Let's go dive into `"app.php"` file and let's see what is going one there.

![app.php file](https://user-images.githubusercontent.com/12012983/76319937-faf11c80-62f0-11ea-94dc-0f59efb5d6dc.png)

There are couple of things do here. It creates new instance of `Application` class with the parameter `$_ENV['APP_BASE_PATH'] ?? dirname(__DIR__)` . So it specified our applications base path. I mean which file location our app instance will be referenced to. In order to do that Application class will work and save applications base path. And it sets our skeleton laravel applications paths. Laravel version defines here. It extends Container class and what Container class basically does is register binds with the container. 
After app instance created we have 3 singleton bindings. 

    - Http\Kernel
        Applications global HTTP middleware stack. We declare any kind of middlewares here.
    - Console\Kernel
        It handles any incoming console commands.
    - Exception\Handler
        Register Handlers.
        
After these bindins it returns app instance, so we can use $app globally.

![applicatoin functions](https://user-images.githubusercontent.com/12012983/76329280-6a6d0900-62fd-11ea-92a8-e2aa7744cb27.png)
 
There are 4 methods in the constructor when we initialize Application class. 

![basepath](https://user-images.githubusercontent.com/12012983/76329448-a4d6a600-62fd-11ea-816e-273d19cc0f20.png)


This method register all relevant paths in the Container, such as applicatoin path, storage path, resource path, etc. 

![paths](https://user-images.githubusercontent.com/12012983/76329744-04cd4c80-62fe-11ea-8258-8ee7e1722692.png)

As we can see all the paths are defined here. It is costumizable to change in the future.

![application](https://user-images.githubusercontent.com/12012983/76330016-5a095e00-62fe-11ea-9bc0-4d9d8a89bbdc.png)


After that our application looks pretty empty. As you can see, nothing has been loaded yet. The container only contains paths. In this moment, any paths can be resolved from the Container













