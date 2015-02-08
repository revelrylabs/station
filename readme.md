## Laravel 5: Backend UI for Content & User Management

Tired of creating and configuring similar models and controllers which deal with basic CRUD, validation and user role-based capabilities? Station allows developers to setup and configure a backend CMS for a Laravel app/site very quickly. 

## Features

* UI using a vanilla bootstrap layout which you can style in your own app.
* Allows for easy table association setup through config files.

## Requirements

* GD (compiled with PHP, if you want to take advantage of the image resizing features)

## Installation 

The Station Service Provider can be installed via [Composer](http://getcomposer.org) by requiring the
`canary/laravel-station` package in your project's `composer.json`.

```json
{
    "require": {
        "canary/laravel-station": "dev-master"
    }
}
```

Then run a composer update
```sh
composer update
```

## Configuration & Setup

This assumes you have a working dev or production environment with Laravel 5 and a database already installed.

### 1. Register Station in app/config/app.php

To use station, you must register the provider when bootstrapping your Laravel application.

Find the `providers` key in your `app/config/app.php` and register the Station Service Provider.

```php
    'providers' => array(
        // ...
        'Illuminate\Html\HtmlServiceProvider',
		'Canary\Station\StationServiceProvider',
        'Way\Generators\GeneratorsServiceProvider',
        'Morrislaptop\LaravelFivePackageBridges\ConfigServiceProvider',
        'Morrislaptop\LaravelFivePackageBridges\Bridges\GeneratorsServiceProvider',
    ),
```

Also update the `aliases` array 

```php 
	'aliases' => [
		// ...
		'Form' 		=> 'Illuminate\Html\FormFacade', 
		'HTML'		=> 'Illuminate\Html\HtmlFacade',
	],
```

Then `bootstrap/app.php` replace

```php
$app = new Illuminate\Foundation\Application(
    realpath(__DIR__.'/../')
);
```

with

```php
$app = new Morrislaptop\LaravelFivePackageBridges\Application(
    realpath(__DIR__.'/../')
);
```

(this is a temporary hack until the Way/Generators package is updated for Laravel 5
)

### 2. Publish Station's assets over to your app.

```sh
php artisan vendor:publish
```

At this time you can (optionally) edit `/app/config/packages/canary/station/_app.php` and change the `root_admin_email`

### 3. Run default migrations if you haven't already. 

`php artisan migrate` 

### 4. Run Station's Build Command. 

This will generate migrations, run migrations, generate models, and seed the database.

```sh
php artisan station:build 
```

### 5. Test Installation

You should now be able to browse to your app at:

http://{host}/station/ (ex. http://app.localhost/station/) and see station running without errors.

You can log in using user/password: `admin/admin`

### 6. Configure Station and Your Panels!

Start by editing `/app/config/packages/canary/station/_app.php`

Then create files for each panel in /app/config/packages/canary/station/ [we need documentation on this]

That's it. You now have a fully functioning back end and user management system for your site.

## Notable Limitations

* The validation rule 'unique' MUST accept 2 parameters: table name, and column name. No more and no less. [ex. unique:users,username]
