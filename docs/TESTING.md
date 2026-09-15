# Testing

## Structure

Unit tests belong to individual custom plugins:

public/wp-content/plugins/
└── umbrella-reading-time/
└── tests/
└── Unit/

Integration tests are project-level:

tests/
└── Integration/

## Adding Unit Tests for a New Plugin

Create:

public/wp-content/plugins/umbrella-example/tests/Unit/

Then register the directory in the root phpunit.xml:

<directory>public/wp-content/plugins/umbrella-example/tests/Unit</directory>

## Running Tests

Run all tests:

docker compose exec \
-e XDEBUG_MODE=off \
-w /var/www/html \
php vendor/bin/phpunit

Run only Unit tests:

docker compose exec \
-e XDEBUG_MODE=off \
-w /var/www/html \
php vendor/bin/phpunit --testsuite Unit

Run only Integration tests:

docker compose exec \
-e XDEBUG_MODE=off \
-w /var/www/html \
php vendor/bin/phpunit --testsuite Integration

PHPUnit uses tests/bootstrap.php, which loads both the project Composer autoloader and WordPress via wp-load.php.