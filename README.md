# NostrGuard
NostrGuard is a universal trust registry and relay infrastructure platform. It manages a volunteer whitelist, enforces invite-chain accountability, and runs a curated Nostr relay that only admits events from registered volunteers.

## Relay (Docker)

```bash
chmod +x scripts/relay_policy.py
docker compose up -d
```

The relay binds to `127.0.0.1:8080`. Reverse-proxy to expose it over TLS (`wss://`).

The write policy plugin (`scripts/relay_policy.py`) runs as a long-lived subprocess inside the container. 
It queries `GET /api/trust/whitelist` at startup and caches the result for 5 minutes. 
No cron job or config reload is needed - whitelist changes take effect within the next cache TTL window.

---

### Seed the first volunteer (bypasses invite requirement)

```bash
docker compose exec backend uv run python -m src.volunteers.trust admin --pubkey  <hex-or-npub>
```

### Configuration

```bash
cp backend/.env.example backend/.env
```
Change the configuration as needed.

---

### Development checks

```bash
cd backend
uv run ruff check --fix
uv run ty check
```
Please run the respective one before submitting a pull request.

## Frontend

```bash
cd frontend
npm install

npm run dev
```
