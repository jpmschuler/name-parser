# THE ICONIC Name Parser

[![Build Status](https://travis-ci.org/theiconic/name-parser.svg?branch=master&t=201705161308)](https://travis-ci.org/theiconic/name-parser)
[![Coverage Status](https://coveralls.io/repos/github/theiconic/name-parser/badge.svg?branch=master&t=201705161308)](https://coveralls.io/github/theiconic/name-parser?branch=master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/theiconic/name-parser/badges/quality-score.png?b=master&t=201705161308)](https://scrutinizer-ci.com/g/theiconic/name-parser/?branch=master)
[![Latest Stable Version](https://poser.pugx.org/theiconic/name-parser/v/stable?t=201705161308)](https://packagist.org/packages/theiconic/name-parser)
[![Total Downloads](https://poser.pugx.org/theiconic/name-parser/downloads?t=201705161308)](https://packagist.org/packages/theiconic/name-parser)
[![License](https://poser.pugx.org/theiconic/name-parser/license?t=201705161308)](https://packagist.org/packages/theiconic/name-parser)

## Purpose
This is a universal, language-independent name parser.

Its purpose is to split a single string containing a full name,
possibly including salutation, initials, suffixes etc., into
meaningful parts like firstname, lastname, initials, and so on.

It is mostly tailored towards english names but works pretty well
with non-english names as long as they use latin spelling.

E.g. **Mr Anthony R Von Fange III** is parsed to
- salutation: **Mr.**
- firstname: **Anthony**
- initials: **R**
- lastname: **von Fange**
- suffix: **III**

## Features

### Supported patterns
This parser is able to handle name patterns with and without comma:
```
... [firstname] ... [lastname] ...
```
```
... [lastname] ..., ... [firstname] ...
```
```
... [lastname] ..., ... [firstname] ..., [suffix]
```

### Supported parts
- salutations (e.g. Mr, Mrs, Dr, etc.)
- first name
- middle names
- initials (single letters, possibly followed by a dot)
- nicknames (parts within parenthesis, brackets etc.)
- last names (also supports prefixes like von, de etc.)
- suffixes (Jr, Senior, 3rd, PhD, etc.)

### Other features
- multi-language support for salutations, suffixes and lastname prefixes
- customizable nickname delimiters
- customizable normalisation of all output strings
  (original values remain accessible)
- customizable whitespace

## Examples

More than 80 different successfully parsed name patterns can be found in the
[parser unit test](https://github.com/theiconic/name-parser/blob/master/tests/ParserTest.php#L12-L12).

## Setup
```$xslt
composer require theiconic/name-parser
```

## Usage

### Basic usage
```php
<?php

$parser = new TheIconic\NameParser\Parser();

$name = $parser->parse($input);

echo $name->getSalutation();
echo $name->getFirstname();
echo $name->getLastname();
echo $name->getMiddlename();
echo $name->getNickname();
echo $name->getInitials();
echo $name->getSuffix();

print_r($name->getAll()); // all parts as an associative array

echo $name; // re-prints the full normalised name
```
An empty string is returned for missing parts.

### Special part retrieval features
#### Full given name
You can get firstname initials and middlename in original order with
```php
echo $name->getFullGivenname();
```
#### Explicit last name parts
You can retrieve last name prefixes and pure last names separately with
```php
echo $name->getLastnamePrefix();
echo $name->getLastname(true); // true enables strict mode for pure lastnames, only
```

#### Nick names with normalized wrapping
By default, `getNickname()` returns the pure string of nick names. However, you can
pass `true` to have the same normalised parenthesis wrapping applied as in `echo $name`:
```php
echo $name->getNickname(); // The Giant
echo $name->getNickname(true); // (The Giant)
```

### Setting Languages
```php
$parser = new TheIconic\NameParser\Parser([
    new TheIconic\NameParser\Language\English(), //default
    new TheIconic\NameParser\Language\German(),
])
```

### Setting nickname delimiters
```php
$parser = new TheIconic\NameParser\Parser();
$parser->setNicknameDelimiters(['(' => ')']);
```

### Setting whitespace characters
```php
$parser = new TheIconic\NameParser\Parser();
$parser->setWhitespace("\t _.");
```

### Limiting the position of salutations
```php
$parser = new TheIconic\NameParser\Parser();
$parser->setMaxSalutationIndex(2);
```
This will require salutations to appear within the
first two words of the given input string.
This defaults to half the amount of words in the input string,
meaning that effectively the salutation may occur within
the first half of the name parts.

## License

THE ICONIC Name Parser library for PHP is released under the MIT License.
