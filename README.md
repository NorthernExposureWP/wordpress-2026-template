# Setup

## 1. Create environment file
cp .env.example .env

## 2. Start Docker
docker compose up -d --build

## 3. Install Composer dependencies
docker compose exec -e XDEBUG_MODE=off php composer install

## 5. Install WordPress

## 6. Verify installation
docker compose exec -e XDEBUG_MODE=off php wp core is-installed --allow-root
(Expected result: 0)
