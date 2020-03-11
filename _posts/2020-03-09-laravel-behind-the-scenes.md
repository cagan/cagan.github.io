---
layout: post
title:  "How Laravel Works Behind the Scenes"
author: "Cagan"
comments: false

---

Everything starts with `index.php` as many web applications. In laravel `index.php` file is stored in `public` folder. 
Let's say we have example application. Once we opened`example.com` our out-of-the-box-configured webserver(Nginx, Apache, ...) points to the `index.php` file located in index.php. This is the initial point that comes up for each request client send. So let's look at the `index.php` file more closely.

```php
<?php

define('LARAVEL_START', microtime(true));

require __DIR__.'/../vendor/autoload.php';

$app = require_once __DIR__.'/../bootstrap/app.php';


$kernel = $app->make(Illuminate\Contracts\Http\Kernel::class);

$response = $kernel->handle(
    $request = Illuminate\Http\Request::capture()
);

$response->send();

$kernel->terminate($request, $response);
```

```php
define('LARAVEL_START', microtime(true));
```

This first time essentialy starts up the timer, so you can time how long it takes to boot up teh framework, etc. But its not using anywhere in Laravel.

```php
require __DIR__.'/../vendor/autoload.php';
```

This line loads the composer autoloader file. Every PHP class is loaded in your project can be used without writing `require ExampleClass.php`

```php
$app = require_once __DIR__.'/../bootstrap/app.php';
```

This is where everything starts to get more interesting. `"app"` instance has been created here. Let's go dive into `"app.php"` file and let's see what is going one there.

```php
$app = new Illuminate\Foundation\Application(
    $_ENV['APP_BASE_PATH'] ?? dirname(__DIR__)
);
$app->singleton(
    Illuminate\Contracts\Http\Kernel::class,
    App\Http\Kernel::class
);

$app->singleton(
    Illuminate\Contracts\Console\Kernel::class,
    App\Console\Kernel::class
);

$app->singleton(
    Illuminate\Contracts\Debug\ExceptionHandler::class,
    App\Exceptions\Handler::class
);

return $app;
```

There are couple of things do here. It creates new instance of `Application` class with the parameter `$_ENV['APP_BASE_PATH'] ?? dirname(__DIR__)` . So it specified our applications base path. I mean which file location our app instance will be referenced to. `$app` is referenced by `Application` class. `Application` class is the class which our app instance is referenced to. Application extends Container class. Purpose of the Container class is binding our application instances with abstract classes or interfaces. (Contextual binding, register new bindings with the container, instantiating a concrete instances). Let's look at Application constructor:

```php
public function __construct($basePath = null)
    {
        if ($basePath) {
            $this->setBasePath($basePath);
        }

        $this->registerBaseBindings();
        $this->registerBaseServiceProviders();
        $this->registerCoreContainerAliases();
    }
```


It check if basePath is given. If yes set base path. Since we get basePath from `__DIR__` it is allways given. So our application directory has been set.

```php
$this->registerBaseBindings();
```
 
 In `registerBaseBindings()` method set instance of the application. Then register an existing instance as shared in the container. So we can access the our app instance as container.
 
```php
registerBaseServiceProviders()
```
 
This method as we can clearly understand from its name it registers some service providers into the container. Basically it registers 3 service providers: EventServiceProvider, LogServiceProvider, RoutingServiceProvider. These are the main providers in order to run laravel system. 
In registerBaseBindings(), we set ourself (ourself = Application class) as a static instance (so we can resolve it using the Singleton design pattern). This is done because we only want ONE global Container. Then, we bind some aliases to ourself, such as app, Container::class (Illuminate\Container\Container) and we also do one specific thing -- binding the package-loader represented by the Illuminate\Foundation\PackageManifest class. Note that this class's constructor does nothing but set up some base paths for itself and store the Filesystem class within itself. No package has been loaded yet. Setting up aliases is done so we can resolve the container from itself by calling app(), app(Container::class) or app('Illuminate\Container\Container').

```php
$this->registerCoreContainerAliases()
```

	



