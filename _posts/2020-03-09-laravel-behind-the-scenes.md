---
layout: post
title:  "How Laravel Works Behind the Scenes"
author: "Cagan"
comments: false

---

Everything starts with `index.php` as many web applications.Once we open `example.com` our out-of-the-box-configured webserver(Nginx, Apache, ...) points to the `index.php` file located in index.php. This is the initial point that comes up for each request client send. So let's look at the `index.php` file more closely.

![index.php](https://user-images.githubusercontent.com/12012983/76318576-f75c9600-62ee-11ea-8b4b-173df294640b.png)