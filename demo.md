Twigony Demo Notes
==================

What we will do:

* Create Symfony project, adding Twigony
* Create login page with user
* Create Entity & Form for NewsPost

# 1. Setting up the project

## 1.1 Do stuff from Twigony's docs:

```sh
$ symfony new twigony-demo
$ composer require timonf/twigony-framework-bundle
```

## 1.2 Enable the Bundle

```php
# AppKernel.php
new Twigony\Bundle\FrameworkBundle\TwigonyFrameworkBundle(),
```

## 1.3 Quick Start

Test your server:

```sh
$ bin/console server:run
```

## 1.4 Start the engines!

* Start PhpStorm with `pstorm .` – **Don't forget to enable Symfony plugin in PhpStorm!**
* Configure your `parameters.yml` and do `bin/console doctrine:database:create`
* Extending the base template!


# 2. User + Login!

## 2.1 Configuring users:

```yml
# app/config/security.yml
security:
    encoders:
        Symfony\Component\Security\Core\User\User: bcrypt
```

## 2.2 Generating a password:

```sh
$ bin/console user:encode-password
```

## 2.2 Extending Security:

```yml
# app/config/security.yml
security:
    providers:
        in_memory:
            memory:
                users:
                    admin:
                        password: ~ # insert generated password here          
                        roles:    ROLE_USER
```

# 3. Adding our Login!

## 3.1 Our first login page…

Let's copy some code from: http://symfony.com/doc/current/security/form_login_setup.html

```twig
{# app/Resources/views/security/login.html.twig #}

{% extends 'base.html.twig' %}

{% block body %}
{% if error %}
    <div>{{ error.messageKey|trans(error.messageData, 'security') }}</div>
{% endif %}

<form action="{{ path('login') }}" method="post">
    <label for="username">Username:</label>
    <input type="text" id="username" name="_username" value="{{ last_username }}" />

    <label for="password">Password:</label>
    <input type="password" id="password" name="_password" />

    {#
        If you want to control the URL the user
        is redirected to on success (more details below)
        <input type="hidden" name="_target_path" value="/account" />
    #}

    <button type="submit">login</button>
</form>
{% endblock %}
```

## 3.2 …with Twigony!

```yml
# app/config/routing.yml
login:
    path: '/login'
    defaults:
        _controller: 'twigony.security_controller:loginAction'
        template: 'security/login.html.twig'
```

Check it and point your Browser to `/login`!

## 3.3 Activate login!

```yml
# app/config/security.yml
security:
    firewalls:
        main:
            anonymous: ~
            form_login:
                login_path: login
                check_path: login
```

# 4. Read the newspaper!

# 4.1 Creating an entity!

Use following things:

* `id`
* `title`
* `content`
* `createdAt`

```php
# src/AppBundle/Entity/NewsPost.php
<?php

namespace AppBundle\Entity;

use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Table(name="news_posts")
 * @ORM\Entity
 */
class NewsPost
{
    /**
     * @ORM\Id()
     * @ORM\GeneratedValue(strategy="AUTO")
     * @ORM\Column(type="integer")
     */
    private $id;

    /**
     * @ORM\Column()
     */
    private $title;

    /**
     * @ORM\Column(type="text")
     */
    private $content;

    /**
     * @ORM\Column(type="datetime")
     */
    private $createdAt;

    public function __construct()
    {
        $this->createdAt = new \DateTime();
    }

    // After here, you can use PhpStorm to generate Getters and Setters! :)

    /**
     * @return mixed
     */
    public function getId()
    {
        return $this->id;
    }

    /**
     * @return mixed
     */
    public function getTitle()
    {
        return $this->title;
    }

    /**
     * @param mixed $title
     */
    public function setTitle($title)
    {
        $this->title = $title;
    }

    /**
     * @return mixed
     */
    public function getContent()
    {
        return $this->content;
    }

    /**
     * @param mixed $content
     */
    public function setContent($content)
    {
        $this->content = $content;
    }

    /**
     * @return mixed
     */
    public function getCreatedAt()
    {
        return $this->createdAt;
    }

    /**
     * @param mixed $createdAt
     */
    public function setCreatedAt($createdAt)
    {
        $this->createdAt = $createdAt;
    }
}
```

Use your database: `bin/console doctrine:schema:update --dump-sql --force`

## 4.2 …and a form!

```php
# src/AppBundle/Form/NewsPostType.php
<?php

namespace AppBundle\Form;

use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\Extension\Core\Type\TextareaType;
use Symfony\Component\Form\FormBuilderInterface;

class NewsPostType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('title')
            ->add('content', TextareaType::class)
        ;
    }
}
```

## 4.3 …and the ugliest form you have ever seen!

```twig
{% extends 'base.html.twig' %}

{% block body %}
    <h2>Add a News post</h2>
    {{ form_start(form) }}
    {{ form_rest(form) }}
    <button type="submit">Save</button>
    {{ form_end(form) }}
{% endblock %}
```

## And now… only Twigony!

```yml
# app/config/routing.yml
news_create:
    path: '/admin/news/create'
    defaults:
        _controller: 'twigony.orm_controller:createAction'
        template: 'news/create.html.twig'
        entity: 'AppBundle\Entity\NewsPost'
        options:
            formClass: 'AppBundle\Form\NewsPostType'
```

## 4.4 Test this shit!

Navigate your browser to `/admin/news/create`

## 4.5 List your news!

Let's start with Twigony first now (order doesn't matter)

```yml
# app/config/routing.yml
news:
    path: '/'
    defaults:
        _controller: 'twigony.orm_controller:listAction'
        template: 'news/index.html.twig'
        entity: 'AppBundle\Entity\NewsPost'
        options:
            orderBy: ['createdAt', 'DESC']
            as: 'posts'
```

And you only need one more thing! (Because you have already your entity)

```twig
{# app/Resources/views/news/index.html.twig #}
{% extends 'base.html.twig' %}

{% block body %}
    <h2>Our News paper!</h2>
    {% for post in posts %}
        <hr>
        <h3>{{ post.title }}</h3>
        <p>
            {{ post.content }}
        </p>
        <em>
            {{ post.createdAt|date('d/m/Y H:i') }}
        </em>
    {% endfor %}
{% endblock %}
```
