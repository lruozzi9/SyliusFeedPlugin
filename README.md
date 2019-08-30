# Sylius Feed Plugin

[![Latest Version][ico-version]][link-packagist]
[![Latest Unstable Version][ico-unstable-version]][link-packagist]
[![Software License][ico-license]](LICENSE)
[![Build Status][ico-travis]][link-travis]
[![Quality Score][ico-code-quality]][link-code-quality]

A plugin for creating all kinds of feeds to any given service. Do you want to create product feeds for
your Google Merchant center? Then this is the right plugin for you.

## Installation

### Step 1: Download the plugin

Open a command console, enter your project directory and execute the following command to download the latest stable version of this plugin:

```bash
$ composer require setono/sylius-feed-plugin
```

This command requires you to have Composer installed globally, as explained in the [installation chapter](https://getcomposer.org/doc/00-intro.md) of the Composer documentation.


### Step 2: Enable the plugin

Then, enable the plugin by adding it to the list of registered plugins/bundles
in the `config/bundles.php` file of your project:

```php
<?php

return [
    // ...
    
    League\FlysystemBundle\FlysystemBundle::class => ['all' => true],
    Setono\SyliusFeedPlugin\SetonoSyliusFeedPlugin::class => ['all' => true],
    
    // It is important to add plugin before the grid bundle
    Sylius\Bundle\GridBundle\SyliusGridBundle::class => ['all' => true],
        
    // ...
];
```

**NOTE** that you must instantiate the plugin before the grid bundle, else you will see an exception like
`You have requested a non-existent parameter "setono_sylius_feed.model.feed.class".`

### Step 3: Import routing

```yaml
# config/routes/setono_sylius_feed.yaml
setono_sylius_feed:
    resource: "@SetonoSyliusFeedPlugin/Resources/config/routing.yaml"
```

### Step 4: Configure plugin

```yaml
# config/packages/setono_sylius_feed.yaml
imports:
    - { resource: "@SetonoSyliusFeedPlugin/Resources/config/app/config.yaml" }
```

### Step 5: Update database schema

Use Doctrine migrations to create a migration file and update the database.

```bash
$ bin/console doctrine:migrations:diff
$ bin/console doctrine:migrations:migrate
```

### Step 6: Using asynchronous transport (optional, but recommended)

All commands in this plugin will extend the [CommandInterface](src/Message/Command/CommandInterface.php).
Therefore you can route all commands easily by adding this to your [Messenger config](https://symfony.com/doc/current/messenger.html#routing-messages-to-a-transport):

```yaml
# config/packages/messenger.yaml
framework:
    messenger:
        routing:
            # Route all command messages to the async transport
            # This presumes that you have already set up an 'async' transport
            # See docs on how to setup a transport like that: https://symfony.com/doc/current/messenger.html#transports-async-queued-messages
            'Setono\SyliusFeedPlugin\Message\Command\CommandInterface': async
```


[ico-version]: https://poser.pugx.org/setono/sylius-feed-plugin/v/stable
[ico-unstable-version]: https://poser.pugx.org/setono/sylius-feed-plugin/v/unstable
[ico-license]: https://poser.pugx.org/setono/sylius-feed-plugin/license
[ico-travis]: https://travis-ci.org/Setono/SyliusFeedPlugin.svg?branch=master
[ico-code-quality]: https://img.shields.io/scrutinizer/g/Setono/SyliusFeedPlugin.svg?style=flat-square

[link-packagist]: https://packagist.org/packages/setono/sylius-feed-plugin
[link-travis]: https://travis-ci.org/Setono/SyliusFeedPlugin
[link-code-quality]: https://scrutinizer-ci.com/g/Setono/SyliusFeedPlugin
