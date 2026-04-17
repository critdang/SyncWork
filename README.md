# SyncWork

![SyncWork Banner](public/temp-banner.png)

SyncWork is a Next.js automation platform with authentication, workflow management, billing, and third-party integrations.

## Tech Stack

- Next.js 14
- React 18
- TypeScript
- Prisma
- PostgreSQL
- Clerk Authentication

## Prerequisites

Before running or deploying, make sure you have:

- Node.js 18+
- npm 9+ (or Bun)
- PostgreSQL database
- Clerk account and API keys

## 1. Clone and Install

```bash
git clone <your-repository-url>
cd SyncWork-production-main
npm install
```

If you prefer Bun:

```bash
bun install
```

## 2. Environment Variables

Create a local environment file:

```bash
cp .env.example .env
```

### Required for app startup

Fill these first:

- NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY
- CLERK_SECRET_KEY
- DATABASE_URL

Example DATABASE_URL:

```env
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/SyncWork?schema=public
```

### Optional integration variables

Set these only if you want the related features:

- Discord: DISCORD_CLIENT_ID, DISCORD_CLIENT_SECRET, DISCORD_TOKEN, DISCORD_PUBLICK_KEY, NEXT_PUBLIC_DISCORD_REDIRECT
- Notion: NOTION_API_SECRET, NOTION_CLIENT_ID, NOTION_REDIRECT_URI, NEXT_PUBLIC_NOTION_AUTH_URL
- Slack: SLACK_SIGNING_SECRET, SLACK_BOT_TOKEN, SLACK_APP_TOKEN, SLACK_CLIENT_ID, SLACK_CLIENT_SECRET, SLACK_REDIRECT_URI, NEXT_PUBLIC_SLACK_REDIRECT
- Google Drive: GOOGLE_CLIENT_ID, GOOGLE_CLIENT_SECRET, OAUTH2_REDIRECT_URI, NGROK_URI
- Stripe: STRIPE_SECRET
- Cron auth: CRON_JOB_KEY

## 3. Setup Database

Push Prisma schema to your PostgreSQL database:

```bash
npx prisma db push
```

## 4. Run in Development

Default script uses HTTPS with a local self-signed certificate:

```bash
npm run dev
```

Open:

- https://localhost:3000

Note: The first HTTPS run may ask for your macOS sudo password to trust local certificates.

If you want HTTP only:

```bash
npx next dev
```

## 5. Build and Run in Production Mode (Local Test)

```bash
npm run build
npm run start
```

## Deploy

### Option A: Deploy to Vercel (Recommended)

1. Push the project to GitHub.
2. Import the repository in Vercel.
3. Set all required environment variables in Vercel Project Settings.
4. Deploy.
5. Run Prisma schema push against your production database:

```bash
npx prisma db push
```

Important: Use your production DATABASE_URL in Vercel.

### Option B: Self-host (Node server)

1. Provision a PostgreSQL instance.
2. Set environment variables on your server.
3. Install dependencies and build:

```bash
npm install
npm run build
```

4. Start server:

```bash
npm run start
```

5. Put a reverse proxy (Nginx/Caddy) in front for HTTPS and domain routing.

## Troubleshooting

### Error: Missing Clerk Secret Key or API Key

Your .env is missing Clerk keys. Add:

- NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY
- CLERK_SECRET_KEY

Then restart dev server.

### Error: Environment variable not found: DATABASE_URL

Add DATABASE_URL to .env, then run:

```bash
npx prisma db push
```

### TypeError reading user.name in Settings/Profile

This has been fixed in code by making profile data initialization null-safe and ensuring a user record exists in database.

## Scripts

- npm run dev: Start development server with experimental HTTPS
- npm run build: Build for production
- npm run start: Start production server
- npm run lint: Run lint checks
