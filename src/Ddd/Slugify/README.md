Slugify, an agnostic slug generator
===================================

Slugify is a library which allows to generate slugs easily whatever persistence
mecanism you use (Propel2, Doctrine2, custom ORM...). To generate a slug of a
string there is always two 2 steps:

- The transliteration step which convert a given string from any writing system
  (French, Deutsh, Greek, Arabic...) into its ASCII representation.
- The slug generation step which basically separate each word and field by a custom delimiter.

Installation
------------

Using Composer, just require the `ddd/slugify` package:

``` javascript
{
    "require": {
        "ddd/slugify": "dev-master"
    }
}
```

Usage
-----

To be able to slugify an entity or model, you just have to implement the `SluggableInterface`:

``` php
<?php

use Ddd\Slugify\Model\SluggableInterface;
use Ddd\Slugify\Service\SlugGeneratorInterface;

class Article implements SluggableInterface
{
    private $createdAt;
    private $title;
    private $slug;

    public function slugify(SlugGeneratorInterface $slugifier)
    {
        $this->slug = $slugifier->slugify(array($this->createdAt->format('Y'), $this->title));
    }

    // other methods...
}
```

Then you just have to call the `slugify` method to generate the slug:

``` php
use Ddd\Slugify\Infra\SlugGenerator\DefaultSlugGenerator;
use Ddd\Slugify\Infra\Transliterator\LatinTransliterator;

$article = new Article();
$article->setTitle('Hello world!');
$article->slugify(new DefaultSlugGenerator(new LatinTransliterator()));

echo $article->getSlug(); // writes "2013-hello-world"
```