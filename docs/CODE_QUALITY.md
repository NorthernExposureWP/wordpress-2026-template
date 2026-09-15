## PHPStan

Run static analysis:

docker compose run --rm \
-e XDEBUG_MODE=off \
-w /var/www/html \
php composer analyse

PHPStan is configured with a 512 MB memory limit because
WordPress analysis can exceed PHP's default 128 MB CLI limit.