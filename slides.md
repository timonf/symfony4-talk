<!-- .slide: data-background="#1ee6fa" -->
# Twigony <!-- with some HTML in this Markdown file, sorry! -->

### Simplify simple pages in Symfony

Timon, Software Developer @ **Interlutions** · [@t1m0n](https://twitter.com/t1m0n)

------
# The way you did small pages before

* One of the 6,879,059+ JavaScript frameworks!?
* Plain HTML <span class="fragment fade-in">– *Hard to grow, no forms, no DB*</span>
* Silex <span class="fragment fade-in">– *Ugly when it gets complex (DB, forms…)*</span>
* Symfony <span class="fragment fade-in">– *To heavy at the beginning*</span>

---
# Plain HTML

* Yeah... as you might know:
* You cannot do anything with plain HTML.
* Not even reuse code (header, footer…)

---
# Using Silex
<span class="fragment fade-in">(personally I hate Silex)</span>
<br><br>

* Need Forms, DB? You might use Symfony* already
* Backend, registration? Too complex!
* If you already have a complex Silex, it's hard to switch to Symfony

*or bundles from the Symfony standard edition framework

---
# Common problems when using Symfony
(on small pages or projects)
<br><br>

* Hard to setup
* Copy-paste from existing projects
* 80 % templates, 15 % reused code, 5 % very new, cool code!
* But: It's super to extend! (APIs, bundles, complex things)

---
# …conclusion:

* Symfony is a good way, but causes problems
  <span class="fragment fade-in">especially on small pages for reused use-cases</span>

------
# Do you know the `TemplateController`?

```yaml
# app/config/routing.yml: (from the official documentation)
acme_privacy:
    path: /privacy
    defaults:
        _controller: FrameworkBundle:Template:template
        template:    static/privacy.html.twig
```

---
# TemplateController: What is it?

* Render static pages <span class="fragment fade-in">**without** any line of PHP!</span>
* You don't need your own controller
* You **avoid** empty controllers like:

  ```php
  public function imprintAction()
  {
      return $this->render('info/imprint.html.twig', []);
  }
  ```

---
<!-- .slide: data-background-image="https://media.giphy.com/media/rWbbNv8E0JBq8/giphy.gif" -->
<!-- .slide: data-background-size="contain" -->
<!-- .slide: data-background-repeat="no-repeat" -->
<!-- .slide: data-class="has-light-background" -->
## Wow!

This is a good idea!

Less PHP for less stuff!

---
## Imagine…

You could do more with this!


------
<!-- .slide: data-background="#1ee6fa" -->
# Twigony

### Twigony Framework Bundle

---
# Introducing Twigony

* Based on the idea of Symfony's TemplateController
* Fast bootstrapping a Symfony project
* Fast prototyping to start a new project

---
## What offers Twigony?

Predefined controllers:
* for static pages (as seen before)
* for sending an email (e.g. contact forms)…
* list entries from DB with pagination (blog?)
* _more to come_

---
## How?

Twigony offers **4** controllers:
<br><br>

* TemplateController (no PHP needed!)
* SwiftMailerController (you only need an `Entity` or `Model` and a `Form`)
* DoctrineORMController (you only need an `Entity` and maybe a `Form`)
* SecurityController (= only login page, same as in the Symfony docs)

---
# Use cases

Landingpages, microsites, small pages using:
* Static pages
* Basic emailing: Contact, submit feedback etc.
* Database: Press releases, simple blogs etc.

------
# Example 1: Contact form

<br>

<span class="fragment fade-in">*A little bit PHP is needed…*</span>

---
 
## Why PHP, I thought Twigony does everything?

* You have to define a model/entity (which fields?)
* You have to create a form/type (which types?)


---
## The entity!

```php
<?php
namespace AppBundle\Entity;
use Symfony\Components\Constraints as Assert;
class Contact {
    /** @var @Assert\NotBlank() */
    public $name;
    /** @var @Assert\Email() */
    public $email;
    /** @var @Assert\NotBlank() */
    public $message;
}
```

(Please use getters + setters!)

---
## The form

```php
<?php
namespace AppBundle\Form;
use Symfony\Component\Form\AbstractForm;
class ContactType extends AbstractForm
{
    public function buildForm()
    {
        $this
            ->add('name')
            ->add('email')
            ->add('message')
        ;
    }
}
```

---

## Enough, PHP! Here's the twig!

```twig
# app/views/base.html.twig
<!doctype html><html><body>
  <h1>My first Twigony page!</h1>
  {% block body %}{% endbody %}
</body></html>
```
and:
```twig
# app/views/page/contact.html.twig
{% extends 'base.html.twig' %}

{% block body %}
  <h2>Schreib mir :)</h2>
  {{ form_start(form) }}
    {{ form_rest(form) }}
    <button type="submit">Yeah, send it!</button>
  {{ form_end(form) }}
{% endblock %}
```

---

## Some more Twig, because we will send an email:

```twig
New mail on your page!

From: {{ entity.name }} <{{ entity.email }}>
Message:

{{ entity.message }}
```

---
## Summary

* You've created a model to store the data (`AppBundle/Entity/Contact`)
* You've created a form to enter the data (`AppBundle/Form/ContactType`)
* You've created your templates (the way you should already know)

---
<!-- .slide: data-background-image="https://media.giphy.com/media/IMuqnp96sdhyE/giphy.gif" -->
<!-- .slide: data-background-size="contain" -->
<!-- .slide: data-background-repeat="no-repeat" -->
<!-- .slide: data-class="has-light-background" -->
## Wait…!

* That's all?
* What is my route?
* Where is my controller?

---
## One more thing

You have to stick them together!

<span class="fragment fade-in">*only* in your `routing.yml`!</span>

---
## This code + Twigony does everything else!

```yaml
# app/config/routing.yml:
app_contact:
    path: /contact
    defaults:
        _controller: twigony.swift_controller:emailAction
        entity:      'AppBundle\Entity\Contact'
        template:    info/contact.html.twig
        options:
            form_class: 'AppBundle\Form\ContactType'
            subject:    'Email from your webpage'
            to:         'myself@example.com'
```

---
## Final summary for the contact form

* You've created a model to store the data (`AppBundle/Entity/Contact`)
* You've created a form to enter the data (`AppBundle/Form/ContactType`)
* You've created your templates (the way you should already know)
* You used Twigony in your `routing.yml` :)

---
## Conclusion

* Only custom code (Entity, Form)
* PHP without deep logic
* Very fast!

---
## Best thing about Twigony

When your project grows or gets more complex

you can *reuse* everything 


------
<!-- .slide: data-background="#1ee6fa" -->
# interlutions.de

Let's see some real world examples

---
## Before (with Silex)

```php
$app->get('/login', function (Request $request) use ($app) {
    return $app['twig']->render('pages/admin/login.html.twig', array(
        'error'         => $app['security.last_error']($request),
        'last_username' => $app['session']->get('_security.last_username')
    ));
})->bind('backend_login');
```

## After (with Twigony)

```yaml
backend_login:
    path: '/login'
    defaults:
        _controller: 'twigony.security_controller:loginAction'
        template: 'pages/admin/login.html.twig'
```

---
## Before (with Silex)

```php
# somewhere in the app.php
$app->get('/team/{team_member}', function (Silex\Application $app, $team_member) use ($app) {
       $dir_templates = __DIR__ . '/../../templates/';
       $page_from_team_member = 'pages/team/' . $team_member . '.html.twig';
   
       if (!file_exists($dir_templates . $page_from_team_member)) {
           $app->abort(404, 'Page does not exist.');
       }
   
       return $app['twig']->render($page_from_team_member, array());
   })->bind('team');
```

## After (with Twigony)

```yaml
team:
    path: '/team/{team_member}'
    defaults:
        template: 'pages/team/{team_member}.html.twig'
        _controller: 'twigony.template_controller:templateAction'
```

------
<!-- .slide: data-background="#1ee6fa" -->
# Demo time?

------
# Good to know…

* You want more features? → [Create a Feature Request](https://github.com/timonf/twigony-bundle/issues/new)
* You need a backend? → [Use EasyAdminBundle](https://github.com/javiereguiluz/EasyAdminBundle)
* You need a user login/registration? → [Use FosUserBundle](https://github.com/FriendsOfSymfony/FOSUserBundle)

------
# Summary

* Twigony is good for starting simple projects
* Twigony is best for simple landingpages

------
<!-- .slide: data-background="#1ee6fa" -->
# Questions?

<span class="fragment fade-in">*Thank you!*</span>
