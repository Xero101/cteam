# Cyber Report Dashboard — Ubuntu deployment

This package contains a fresh production build and a consistent snapshot of the current reports database. It does not contain the source machine's `.env` or `node_modules`.

## Requirements

- Ubuntu Server with Node.js 22.13+ (Node.js 24 LTS recommended; the app uses `node:sqlite`)
- pnpm 10+

## Install

1. Copy and extract this ZIP to a persistent directory, for example `/opt/cyber-report-dashboard`.
2. In that directory, install pnpm if needed: `sudo npm install --global pnpm@10`.
3. Install dependencies: `pnpm install --prod --frozen-lockfile`.
4. Create the local settings file and set your own credentials:

   ```sh
   cp .env.example .env
   nano .env
   ```

   Create a session secret with `openssl rand -hex 32`. Replace both example admin credentials and the session secret before starting the app.
5. Start it with `pnpm start`.

The app listens on port 3001. `HOST=0.0.0.0` makes it reachable on the server network; use a firewall and preferably a reverse proxy with HTTPS before exposing it publicly. For a reverse proxy on this same server, set `HOST=127.0.0.1` in `.env` instead.

## Data and updates

The included `data/reports.sqlite` is a consistent snapshot of the current reports. The app will create the data directory if missing. Keep `data/reports.sqlite` on persistent storage and back it up before replacing a deployment. To deploy a later package while preserving reports, copy the existing database into the new package's `data/` directory before starting it. Keep `.env` private and preserve it across updates.
