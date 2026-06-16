# ig-artology

Automated Instagram promotion bot for the [Artology Shopee store](https://shopee.co.id/artology).

The application fetches active product listings from the Artology Shopee store and publishes a daily promotional post to the Artology Instagram account via the Meta Graph API.

## What it does

1. **Scrapes / fetches listings** from `shopee.co.id/artology` (products, images, prices, descriptions).
2. **Selects a product** to promote each day (e.g. newest, random, or manually queued).
3. **Generates a caption** from the product data.
4. **Publishes a post** to Instagram using the Meta (Facebook) Graph API.
5. **Logs each post** so the same listing is not promoted twice in a row.

A Laravel scheduler task runs daily and drives the whole pipeline automatically.

## Tech stack

| Layer | Technology |
|---|---|
| Backend | Laravel 13, PHP 8.5 |
| Frontend | React 19, Inertia.js v3, TailwindCSS v4 |
| Database | SQLite (local) |
| Auth | Laravel Fortify (2FA, passkeys) |
| Scheduling | Laravel Scheduler (`app/Console/Kernel.php` or `routes/console.php`) |
| Instagram | Meta Graph API (Instagram Basic Display / Content Publishing) |
| Shopee data | Shopee Open Platform API or HTTP scraping |

## Requirements

- PHP 8.5
- Composer
- Node.js + npm
- A [Meta Developer App](https://developers.facebook.com/) with Instagram Graph API access
- An Instagram Professional account connected to a Facebook Page
- (Optional) Shopee Open Platform API credentials

## Setup

```bash
# Install PHP dependencies
composer install

# Install JS dependencies
npm install

# Copy environment file
cp .env.example .env

# Generate app key
php artisan key:generate

# Run migrations
php artisan migrate

# Start development server
composer run dev
```

The app will be available at **http://localhost:8000**.

## Environment variables

See `.env.example` for all required variables. The critical ones for the automation pipeline are:

```
# Instagram / Meta Graph API
INSTAGRAM_ACCOUNT_ID=
META_APP_ID=
META_APP_SECRET=
META_ACCESS_TOKEN=        # long-lived page access token

# Shopee (optional — only if using the Open Platform API)
SHOPEE_PARTNER_ID=
SHOPEE_PARTNER_KEY=
SHOPEE_SHOP_ID=
SHOPEE_ACCESS_TOKEN=

# Artology store slug
ARTOLOGY_SHOPEE_URL=https://shopee.co.id/artology
```

## Running the scheduler

In production, add a single cron entry to run the Laravel scheduler every minute:

```
* * * * * cd /path/to/ig-artology && php artisan schedule:run >> /dev/null 2>&1
```

Locally you can run:

```bash
php artisan schedule:work
```

## Deployment

Deploy to [Laravel Cloud](https://cloud.laravel.com) for managed hosting with built-in scheduler support.
