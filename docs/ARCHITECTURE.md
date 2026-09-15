## Project-level Composer Autoloading

Composer is configured at the **project level**, not inside individual custom plugins.

The project `composer.json` contains PSR-4 mappings for custom plugins, for example:

```json
"autoload": {
    "psr-4": {
        "Umbrella\\ReadingTime\\": "public/wp-content/plugins/umbrella-reading-time/"
    }
}
```

The project-level Composer autoloader is loaded from `public/wp-config.php`:

```php
require_once dirname(__DIR__) . '/vendor/autoload.php';
```

This is required because WP-CLI uses its **own Composer autoloader** inside the `wp-cli.phar`. It does not automatically use the project's `vendor/autoload.php`.

Therefore, without the `require_once` above:

```bash
./wpcli eval 'var_dump(class_exists(\Umbrella\ReadingTime\ReadingTimeCalculator::class));'
```

may return:

```text
bool(false)
```

even though the project's Composer autoloader works correctly.

After adding or changing a PSR-4 mapping in `composer.json`, regenerate the Composer autoloader:

```bash
docker compose run --rm \
    -e XDEBUG_MODE=off \
    -w /var/www/html \
    php composer dump-autoload
```

Then verify that the project's class can be autoloaded:

```bash
./wpcli eval 'var_dump(class_exists(\Umbrella\ReadingTime\ReadingTimeCalculator::class));'
```

Expected result:

```text
bool(true)
```

### Important rule for custom plugins

Custom plugins **must not** contain their own `vendor/` directory or load Composer using:

```php
require_once __DIR__ . '/vendor/autoload.php';
```

The plugin relies on the project-level Composer autoloader being loaded by `wp-config.php`.

When adding a new custom `umbrella-*` plugin:

1. Add its PSR-4 namespace to the root `composer.json`.
2. Run `composer dump-autoload`.
3. Make sure the project-level autoloader is loaded from `wp-config.php`.
4. The plugin can then use its namespaced classes without a local Composer installation.
