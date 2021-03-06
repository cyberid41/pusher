Laravel Pusher
==============

![image](https://raw.githubusercontent.com/vinkla/vinkla.github.io/master/images/laravel-pusher.png)

Laravel [Pusher](https://pusher.com/) is a [Pusher](https://pusher.com/) bridge for Laravel 5 using the [official Pusher package](https://github.com/pusher/pusher-php-server).

[![Build Status](https://img.shields.io/travis/vinkla/pusher/master.svg?style=flat)](https://travis-ci.org/vinkla/pusher)
[![Latest Stable Version](http://img.shields.io/packagist/v/vinkla/pusher.svg?style=flat)](https://packagist.org/packages/vinkla/pusher)
[![License](https://img.shields.io/packagist/l/vinkla/pusher.svg?style=flat)](https://packagist.org/packages/vinkla/pusher)

## Installation
Require this package, with [Composer](https://getcomposer.org/), in the root directory of your project.

```bash
composer require "vinkla/pusher=~1.0"
```

Add the service provider to ```config/app.php``` in the `providers` array.

```php
'Vinkla\Pusher\PusherServiceProvider'
```

If you want you can use the [facade](http://laravel.com/docs/facades). Add the reference in ```config/app.php``` to your aliases array.

```php
'Pusher' => 'Vinkla\Pusher\Facades\Pusher'
```

#### Looking for a Laravel 4 compatable version?

Please use [@artdarek's Laravel 4 Pusherer package](https://github.com/artdarek/pusherer) instead.

## Configuration

This will create a `config/pusher.php` file in your app that you can modify to set your configuration. Also, make sure you check for changes to the original config file in this package between releases.

#### Default Connection Name

This option `default` is where you may specify which of the connections below you wish to use as your default connection for all work. Of course, you may use many connections at once using the manager class. The default value for this setting is `main`.

#### Pusher Connections

This option `connections` is where each of the connections are setup for your application. Example configuration has been included, but you may add as many connections as you would like.

## Usage

#### PusherManager

This is the class of most interest. It is bound to the ioc container as `pusher` and can be accessed using the `Facades\Pusher` facade. This class implements the ManagerInterface by extending AbstractManager. The interface and abstract class are both part of [@GrahamCampbell Laravel Manager](https://github.com/GrahamCampbell/Laravel-Manager) package, so you may want to go and checkout the docs for how to use the manager class over at that repository. Note that the connection class returned will always be an instance of `Pusher`.

#### Facades\Pusher

This facade will dynamically pass static method calls to the `pusher` object in the ioc container which by default is the `PusherManager` class.

#### PusherServiceProvider

This class contains no public methods of interest. This class should be added to the providers array in `config/app.php`. This class will setup ioc bindings.

### Examples
Here you can see an example of just how simple this package is to use. Out of the box, the default adapter is `main`. After you enter your authentication details in the config file, it will just work:

```php
// You can alias this in config/app.php.
use Vinkla\Pusher\Facades\Pusher;

Pusher::trigger('my-channel', 'my-event', ['message' => $message]);
// We're done here - how easy was that, it just works!

Pusher::getSettings();
// This example is simple and there are far more methods available.
```

The Pusher manager will behave like it is a `Pusher`. If you want to call specific connections, you can do that with the connection method:

```php
use Vinkla\Pusher\Facades\Pusher;

// Writing this…
Pusher::connection('main')->log('They see me logging…');

// …is identical to writing this
Pusher::log('They see me logging…');

// and is also identical to writing this.
Pusher::connection()->log('They see me logging…');

// This is because the main connection is configured to be the default.
Pusher::getDefaultConnection(); // This will return main.

// We can change the default connection.
Pusher::setDefaultConnection('alternative'); // The default is now alternative.
```

If you prefer to use dependency injection over facades like me, then you can inject the manager:

```php
use Vinkla\Pusher\PusherManager;

class Foo {
	protected $pusher;

	public function __construct(PusherManager $pusher)
	{
		$this->pusher = $pusher;
	}

	public function bar()
	{
		$this->pusher->trigger('my-channel', 'my-event', ['message' => $message]);
	}
}

App::make('Foo')->bar();
```

## Documentation
There are other classes in this package that are not documented here. This is because the package is a Laravel wrapper of [the official Pusher package](https://github.com/pusher/pusher-php-server).

## License

The Laravel Pusher package is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).
