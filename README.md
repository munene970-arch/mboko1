# Digits Trading App

A self-hosted digit trading app built on the Deriv WebSocket API. Supports Matches/Differs, Over/Under, and Even/Odd contract types with real-time tick streaming and digit frequency statistics.

## Prerequisites

- Node.js 18.18 or later

## Step 1: Register Your App ID

1. Log in to your Deriv account and go to the [API Token page](https://app.deriv.com/account/api-token) to create a token with the required scopes.
2. Navigate to [App Registration](https://developers.deriv.com/dashboard/) and register a new application.
3. Set the **Redirect URI** to the URL where you will host this app (e.g. `http://localhost:3000` for local development).
4. Copy the **App ID** shown after registration — you will need it in the next step.

## Step 2: Configure `.env.production`

Copy `.env.example` to `.env.production` and fill in your values:

```bash
cp .env.example .env.production
```

Edit `.env.production`:

```env
NEXT_PUBLIC_DERIV_APP_ID=your_app_id_here
NEXT_PUBLIC_DERIV_REDIRECT_URI=https://your-registered-redirect-uri.com
NEXT_PUBLIC_DERIV_APP_NAME=your_app_name_here
NEXT_PUBLIC_DERIV_REFERRAL_LINK=your_referral_link_here
NEXT_PUBLIC_DERIV_OAUTH_SCOPES=trade,account_manage
NEXT_PUBLIC_DERIV_ENV=production
```

| Variable | Description |
|---|---|
| `NEXT_PUBLIC_DERIV_APP_ID` | Your Deriv app ID from the App Registration dashboard |
| `NEXT_PUBLIC_DERIV_REDIRECT_URI` | OAuth redirect URI — must exactly match the URI registered in your Deriv app |
| `NEXT_PUBLIC_DERIV_APP_NAME` | App name shown in the header |
| `NEXT_PUBLIC_DERIV_REFERRAL_LINK` | Affiliate referral link shown to unauthenticated users (optional) |
| `NEXT_PUBLIC_DERIV_OAUTH_SCOPES` | Comma-separated OAuth scopes (e.g. `trade,account_manage`) |
| `NEXT_PUBLIC_DERIV_ENV` | `production` to connect to the live Deriv endpoint; `preview` for staging |

## Step 2b: Set `NETLIFY_AUTH_TOKEN` for CI / scripted deploys

For CI or machines without a local Netlify login, set `NETLIFY_AUTH_TOKEN` before running deploy commands.

On macOS/Linux:

```bash
export NETLIFY_AUTH_TOKEN=your_netlify_auth_token
npm run deploy
```

On Windows PowerShell:

```powershell
$env:NETLIFY_AUTH_TOKEN = 'your_netlify_auth_token'
npm run deploy
```

To create a token:

1. Open https://app.netlify.com/user/applications#personal-access-tokens
2. Create a new personal access token with `sites:read` and `deployments:write` permissions.
3. Use that token in `NETLIFY_AUTH_TOKEN`.

## GitHub Actions CI deploy

This repo includes a GitHub Actions workflow at `.github/workflows/deploy.yml`.

To use it, add these repository secrets in GitHub:

- `NETLIFY_AUTH_TOKEN` — your Netlify personal access token
- `NETLIFY_SITE_ID` — your Netlify site ID (`ac42b83c-5f8f-48da-bc23-c46004ba38ba`)

The workflow runs on pushes to `main` and can also be triggered manually.

For local development, copy `.env.production` to `.env.local` — Next.js will load `.env.local` automatically and it takes precedence over `.env.production`.

## Step 3: Local Development

```bash
npm install
npm run dev
```

The app is available at `http://localhost:3000`.

## Step 4: Build for Production

```bash
npm run build
```

This produces a fully static export in the `/out` directory. Serve the contents of `/out` from any web server or static file host.
