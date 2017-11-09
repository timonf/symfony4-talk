<!-- .slide: data-background="#1ee6fa" -->
# Symfony 4 <!-- with some HTML in this Markdown file, sorry! -->

### Discovering Symfony 4 as a new framework

Timon, Software Developer @ **Interlutions** · [@t1m0n](https://twitter.com/t1m0n)

---
# What is Symfony? (the way you maybe know it)

---
# It's big!

---
# It's really big!

* `> 33 MB` after initial setup.
* Comes with Doctrine, Swiftmailer, Twig
* Comes with complex Debug tools and web server

---
# It's easy to extend

---
but

---
# It's hard to remove unnecessary sh**t!

---
# …conclusion:

* Symfony is sh**t<span class="fragment fade-in">?</span>

---
# There is something…

------
<!-- .slide: data-background="#1ee6fa" -->
# Symfony <span class="fragment fade-in">4</span>

---
## Discovering Symfony 4 as a new framework

---
## Requirements

* PHP 7.1
* Composer

---
## You don't need

* A database
* A framework specific CLI

---
## A Symfony 4 project consists of...

* Composer
* Symfony Flex
* Minimalistic Symfony setup
* Much configuration 

------
<!-- .slide: data-background="#1ee6fa" -->
# Symfony Flex

---
## What it is?

* a Composer plugin
* which you don't need to install
* …it comes with the project template (aka "skeleton")

---
## Create project using Flex

```sh
$ composer create-project symfony/skeleton helloworld    
```

---
### or for now:

```sh
$ composer create-project \
    symfony/skeleton:^4.0@beta \
    helloworld
``` 

---
## What will we get?

---
```
helloworld
├── bin
│   └── console
├── config
│   ├── bundles.php
│   ├── packages
│   │   ├── framework.yaml
│   │   └── routing.yaml
│   ├── routes.yaml
│   └── services.yaml
├── public
│   └── index.php
├── src
│   └── Kernel.php
├── composer.json
└── symfony.lock
```

---
## Yeah, much configuration! Eh... why?

* More modular. Easily add or remove configs!
* Symfony Flex can easier add or remove plugins (aka bundles)
* ...and it's transparent. Look at the `AppKernel.php` file!

------
# What else is Symfony 4?

* Powerful framework
* ...with a powerful Dependency Injection system
* ...easy and flexible to extend

---
## Extending Symfony

* Need DB? → `composer req doctrine`
* Need template engine? → `composer req twig`
* Need an admin interface? → `composer req admin`
* Discover more at [https://symfony.sh](symfony.sh)

------
<!-- .slide: data-background="#1ee6fa" -->
# Practice time

---
## Creating the project

```
$ composer create-project \
    symfony/skeleton:^4.0@beta helloworld
```

---
## Creating a controller

* Introducing parameter injection
* Introducing annotations (powerful!)

---
## Let's build a controller

---
## Enable annotation routing

```yaml
controllers:
    resource: ../src/Controller/
    type: annotation
```

---
## Integrating Twig

```
$ composer req twig
```

---
## ...and we could do more!

------
<!-- .slide: data-background="#1ee6fa" -->
# Summary

---
# The good

* With Flex Symfony is more flexible
* Very compact

---
# The bad

* Hard to setup a standard setup consists
  of a template engine, database & more.
* Much to configure

---
## Purposes

* Custom projects
* Small projects
* Static projects
* Growing projects

------
<!-- .slide: data-background="#1ee6fa" -->
# Questions?

<span class="fragment fade-in">*Thank you!*</span>
