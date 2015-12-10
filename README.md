# router

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE.md)
[![Build Status][ico-travis]][link-travis]
[![Code Climate][ico-cc]][link-cc]
[![Tests Coverage][ico-cc-coverage]][link-cc]

A simple request router.

## Install

Via Composer

``` bash
$ composer require vakata/router
```

## Usage

``` php
// create an instance
$router = new \vakata\router\Router();
$router
    ->get('/', function () { echo 'homepage'; })
    ->get('/profile', function () { echo 'user profile'; })
    ->with('/books/') // specify a prefix for all future routes
        ->get('read/{i:id}', function ($matches) {
            // this method uses a named placeholder
            // provided the user visits /books/read/10 matches will contain:
            var_dump($matches); // 0 => books, 1 => read, 2 => 10, id => 10
            // placeholders are wrapped in curly braces {...} and can be: 
            //  - i - an integer
            //  - a - any letter (a-z)
            //  - h - any letter or integer
            //  - * - anything (up to the next slash (/))
            //  - ** - anything (to the end of the URL)

            // placeholders can be named too by using the syntax:
            // {placeholder:name}
            
            // placeholders can also be optional
            // {?optional}
        })
        // for advanced users - you can use any regex as a placeholder:
        ->get('{(delete|update):action}/{(\d+):id}', function ($matches) { })
        // you can also use any HTTP verb
        ->post('delete/{i:id}', function ($matches) { })
    // here is how you reset the prefix
    ->with('')
        // you can also bind multiple HTTP verbs in one go
        ->add(['GET', 'HEAD'], '/path', function () { })
    // you can also use with() statements to execute some code if the begging of the URL is a match to the prefix
    ->with('user', function () { echo 1; })
        ->get('view', function () { /* 1 will be echoed */ })
        ->post('chat', function () { /* 1 will be echoed */ });

// you finally run the router
try {
    $router->run($_SERVER['REQUEST_URI'], $_SERVER['REQUEST_METHOD']);
} catch (\vakata\router\RouterException $e) {
    // thrown if no matching route is found
}
```

## Testing

``` bash
$ composer test
```


## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security

If you discover any security related issues, please email github@vakata.com instead of using the issue tracker.

## Credits

- [vakata][link-author]
- [All Contributors][link-contributors]

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

[ico-version]: https://img.shields.io/packagist/v/vakata/router.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-travis]: https://img.shields.io/travis/vakata/router/master.svg?style=flat-square
[ico-scrutinizer]: https://img.shields.io/scrutinizer/coverage/g/vakata/router.svg?style=flat-square
[ico-code-quality]: https://img.shields.io/scrutinizer/g/vakata/router.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/vakata/router.svg?style=flat-square
[ico-cc]: https://img.shields.io/codeclimate/github/vakata/router.svg?style=flat-square
[ico-cc-coverage]: https://img.shields.io/codeclimate/coverage/github/vakata/router.svg?style=flat-square

[link-packagist]: https://packagist.org/packages/vakata/router
[link-travis]: https://travis-ci.org/vakata/router
[link-scrutinizer]: https://scrutinizer-ci.com/g/vakata/router/code-structure
[link-code-quality]: https://scrutinizer-ci.com/g/vakata/router
[link-downloads]: https://packagist.org/packages/vakata/router
[link-author]: https://github.com/vakata
[link-contributors]: ../../contributors
[link-cc]: https://codeclimate.com/github/vakata/router

