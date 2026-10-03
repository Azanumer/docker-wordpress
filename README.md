# Docker WordPress

Production-style WordPress stack in one file: **WordPress + MariaDB + Redis + phpMyAdmin**.

## Quick start

```bash
cp .env.example .env          # set strong passwords first
docker compose up -d
```

- Site → `http://localhost:8080` (finish the WP installer)
- phpMyAdmin → `http://localhost:8081` (login with `DB_USER` / `DB_PASSWORD`)

## What's inside

| Service | Image | Notes |
|---|---|---|
| `wordpress` | wordpress:6-php8.2 | Files persisted in the `wp_data` volume |
| `db` | mariadb:10.11 | Data persisted in the `db_data` volume |
| `redis` | redis:7-alpine | 128MB, LRU — for the Redis Object Cache plugin |
| `phpmyadmin` | phpmyadmin/phpmyadmin | 256M upload limit for big DB imports |

## Tips

- **Object cache:** install the "Redis Object Cache" plugin, set host to `redis`, port `6379` — instant TTFB drop.
- **Backups:** `docker compose exec db mariadb-dump -u root -p"$DB_ROOT_PASSWORD" "$DB_NAME" > backup.sql` (volumes hold the rest).
- **Live code edits:** uncomment the `./wp-content` bind mount to develop a theme/plugin straight from your machine.
- **Stopping:** `docker compose down` keeps volumes; `docker compose down -v` wipes everything (database included).

MIT licensed.
