# docker-symfony

Docker-symfony gives everything we need for developing Symfony application. This complete stack run with docker and docker-compose (1.23 or higher).

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
    - [Install DoctrineMigrationsBundle](#install-doctrinemigrationsbundle)
    - [Install DoctrineFixturesBundle](#install-doctrinefixturesbundle)
- [Development](#development)
    - [Using database migration](#using-database-migration)
- [Troubleshooting](#troubleshooting)
    - [Known issues](#known-issues)
- [Resources](resources)
    
## Prerequisites

You need to make sure that you have  `docker` installed

```
$ which docker
/usr/local/bin/docker
```

There is no other prerequisite needed in order to setup this project for development.

## Installation

1. Create a `.env` from the `.env.dist` file. Adapt it according to your symfony application

```bash
cp .env.dist .env
```
    
2. Build/run containers with (with and without detached mode)

```bash
docker-compose build
docker-compose up -d
```
    
3. Install Symfony

```bash
docker-compose exec php bash
./install-symfony.sh
```

[[table of contents]](#table-of-contents)

### Install DoctrineMigrationsBundle

First, install the bundle with composer:

```bash
docker-compose exec php bash
composer require doctrine/doctrine-migrations-bundle
```

If everything worked, the DoctrineMigrationsBundle can now be found at vendor/doctrine/doctrine-migrations-bundle.

Finally, be sure to enable the bundle in AppKernel.php by including the following:

```php
// app/AppKernel.php
public function registerBundles()
{
    $bundles = array(
        //...
        new Doctrine\Bundle\MigrationsBundle\DoctrineMigrationsBundle(),
    );
}
```
    
[[table of contents]](#table-of-contents)

### Install DoctrineFixturesBundle

First, install the bundle with composer:

```bash
docker-compose exec php bash
composer require --dev doctrine/doctrine-fixtures-bundle nelmio/alice ^2.1.4
```

If everything worked, the DoctrineFixturesBundle can now be found at vendor/doctrine/doctrine-fixtures-bundle and vendor/nelmio/alice.

Finally, be sure to enable the bundle in AppKernel.php by including the following:

```php
// app/AppKernel.php

// ...
// registerBundles()
if (in_array($this->getEnvironment(), ['dev', 'test'], true)) {
    // ...
    $bundles[] = new Doctrine\Bundle\FixturesBundle\DoctrineFixturesBundle();
}
```
    
[[table of contents]](#table-of-contents)

## Development

### Using database migration

First of all [Install DoctrineMigrationsBundle](#install-doctrinemigrationsbundle)

Every time that a change is done into the domain Entity, run this commands:

```bash
docker-compose exec php bash
# To create the migration file
sf3 doctrine:migration:diff
# To run migrations
sf3 doctrine:migration:migrate
``` 
    
[[table of contents]](#table-of-contents)

### Writing fixtures

First of all [Install DoctrineFixturesBundle](#install-doctrinefixturesbundle)

Then create a data fixture

```php
// src/DataFixtures/ORM/AppFixtures.php
namespace AppBundle\DataFixtures\ORM;

use Doctrine\Bundle\FixturesBundle\Fixture;
use Doctrine\Common\Persistence\ObjectManager;
use Nelmio\Alice\Fixtures;

class AppFixtures extends Fixture
{
    public function load(ObjectManager $manager)
    {
        Fixtures::load(__DIR__.'/fixtures.yml', $manager);
    }
}
```

After that create a file `src/DataFixtures/ORM/fixtures.yml` and add fixtures following the documentation [fzaninotto/Faker](https://github.com/fzaninotto/Faker)

Once your fixtures are ready, run the command:

```bash
sf3 doctrine:fixtures:load
```
    
[[table of contents]](#table-of-contents)

## Troubleshooting

### Known issues

There are no known issues.

[[table of contents]](#table-of-contents)

## Resources

[[table of contents]](#table-of-contents)
