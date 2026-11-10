<p align="center"><img src="assets/banner.png" alt="ZscriptBot banner" width="100%" /></p>

# ZscriptBot

**Discord operations in one place: support tickets, product licensing, subscriptions and scheduled role management.**

ZscriptBot connects community support to digital-product administration. It combines Discord interactions with a relational database and background jobs, with clear setup and local development instructions below.

**Explore:** [Features](#features) · [Commands](#commands) · [Getting started](#getting-started) · [Project structure](#project-structure)

## Features

- **Ticket system.** Support, sales, collaboration and custom ticket categories, automatic transcripts and permission handling.
- **Products and orders.** Show a product catalog in Discord, sell license keys and deliver them automatically. Orders can be cancelled or refunded.
- **Subscriptions.** Grant a role for a fixed period, then extend, freeze or cancel it. Scheduled jobs warn users before expiry and remove the role when a subscription ends.
- **License key pool.** Keys are generated and tracked per product.
- **In-Discord setup.** An admin configures ticket categories, archive channels and other settings with the `/kurulum panel` command.
- **Reliable infrastructure.** TypeScript, discord.js v14, PostgreSQL with Prisma, Redis with BullMQ for background jobs, and a Docker Compose setup.

## Commands

The command names are in Turkish, as the bot serves a Turkish community.

| Command | Who | Purpose |
|---------|-----|---------|
| `/ürünler` | Everyone | List active products |
| `/siparişler` | Everyone | Show your orders |
| `/kurulum panel`, `/kurulum durum` | Admin | Open the setup panel, show configuration status |
| `/ürün-ekle`, `/ürün-durum`, `/ürün-sil` | Admin | Create a product, set it active, passive or archived, delete it |
| `/abone-ver`, `/abone-uzat`, `/abone-dondur`, `/abone-iptal`, `/abone-bilgi` | Admin | Grant, extend, freeze, cancel and inspect subscriptions |
| `/sipariş-iptal`, `/sipariş-iade` | Admin | Cancel or refund an order |
| `/sohbet-temizle` | Admin | Delete up to 100 messages |

## Getting started

### Requirements

- Node.js 22 or later
- Docker and Docker Compose
- A Discord bot token and client ID

### Run with Docker

```bash
git clone https://github.com/Burak-ML/ZscriptBot.git
cd ZscriptBot
cp .env.example .env
```

Fill in `.env`:

```env
DISCORD_TOKEN=your_bot_token
CLIENT_ID=your_application_id
DATABASE_URL=postgresql://zscript:zscript@postgres:5432/zscript
REDIS_URL=redis://redis:6379
GUILD_ID=your_server_id   # optional, makes slash commands appear immediately
```

Change the default database password before using this outside local development, and keep it in sync with `docker-compose.yml`.

```bash
docker compose up --build -d
```

Then invite the bot to your server and run `/kurulum panel` as an administrator to choose the channels and categories it should use.

### Local development

```bash
npm install            # also generates the Prisma client
npm run prisma:dev     # create and apply migrations
npm run dev            # start with live reload
```

| Script | Purpose |
|--------|---------|
| `npm run build` / `npm start` | Compile and run the production build |
| `npm run typecheck` / `npm run lint` | Type check and lint |
| `npm test` | Run the Vitest suite |
| `npm run prisma:migrate` | Apply migrations in deployment |

## Project structure

```
src/
  bot/        client, events, slash command definitions and interaction routing
  modules/    tickets, products, orders, payments, licenses, subscriptions, roles, setup, welcome, logs
  services/   shared services (IDs, license keys, permissions, guild settings)
  jobs/       BullMQ jobs for subscription warnings and expiry
  lib/        embeds, errors, locks, Prisma and Redis clients
prisma/       schema and migrations
```

## License

MIT. See [LICENSE](LICENSE).
