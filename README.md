# Setup

## 1. Create environment file

```bash
cp .env.example .env
```

## 2. Start Docker

```bash
docker compose up -d --build
```

## 3. Install Composer dependencies

```bash
docker compose exec -e XDEBUG_MODE=off php composer install
```

## 4. Configure Composer autoload

Add to `public/wp-config.php`:

```php
require_once dirname(__DIR__) . '/vendor/autoload.php';
```

This loads the project-level Composer autoloader before WordPress loads plugins.

## 5. Install WordPress

Follow the WordPress installation in the browser:

```text
http://localhost:8080
```

## 6. Verify installation

```bash
docker compose exec -e XDEBUG_MODE=off php wp core is-installed --allow-root
```

Expected result:

```text
0
```

## 7. Fix file ownership

If files inside `public/` were created by the Docker container and are not writable by your host user:

```bash
sudo chown -R $USER:$USER public/
```

## WP-CLI

Use the project wrapper:

```bash
./wpcli core version
./wpcli plugin list
./wpcli theme list
```

## Composer autoload

After adding or changing a custom plugin namespace in `composer.json`:

```bash
docker compose exec -e XDEBUG_MODE=off php composer dump-autoload
```

Custom plugins use the project-level Composer autoloader and must not contain their own `composer.json` or `vendor/` directory.
