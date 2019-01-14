# Auto No Follow

This simple PHP package has one simple purpose; to take some HTML (as a string), go through it and add `rel="nofollow"` to all links, subject to certain exceptions.

So give it this:

```html
<p>Check out our <a href="http://example.com">affordable web design services</a>!</p>
```

...and you get this:

```html
<p>Check out our <a href="http://example.com" rel="nofollow">affordable web design services</a>!</p>
```

By default, it will ignore relative links, so this would remain untouched:

```html
<p>Check out the <a href="/about">about</a> page</p>
```

It will also ignore absolute links to the current domain.

You can also provide a whitelist of domains, and it will skip them.

## Installation

```bash
composer require lukaswhite/auto-no-follow
```

## Usage

Create an instance:

```php
$processor = new \Lukaswhite\AutoNoFollow\Processor();
```

Optionally specify the current host:

```php
$processor->setCurrentHost('example.com');    
```

> If you don't set this, it'll grab it from `$_SERVER['HTTP_HOST']`

Then run:

```php
$html = $processor->addLinks('<p>Check out our <a href="http://example.com">affordable web design services</a>!</p>');
```

The method signature is this:

```php
public function addLinks($html, $whitelist = array(), $ignoreRelative = true)
```

So, to whitelist certain hosts:

```php
$html = $processor->addLinks(
  '<p>Check out our <a href="http://example.com">affordable web design services</a>!</p>'
  [
  	'example.com'
  ]
);
```

To add `nofollow` to relative links anyway, set the third argument to true, e.g.:

```php
$html = $processor->addLinks(
  '<p>Check out the <a href="/about">about</a> page</p>'
  [],
  TRUE
);
```

This will output the following:

```html
<p>Check out the <a href="/about" rel="nofollow">about</a> page</p>
```

## Tests

The package contains tests, to run them:

```bash
./vendor/bin/phpunit
```
