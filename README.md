# PowerGuardian Docker

PowerGuardian is a UPS monitoring and management controller — inspired by UniFi, built for power infrastructure. This repository provides the official Docker distribution.

## Quick Start (Portainer)

Copy the stack below into Portainer → Stacks → Add Stack.

```yaml
services:
  powerguardian:
    image: ghcr.io/powerguardianos/powerguardian-docker:latest
    container_name: powerguardian
    restart: unless-stopped
    ports:
      - "8181:8181"
    volumes:
      - powerguardian-data:/data
    environment:
      PG_ADDR: "0.0.0.0:8181"
      PG_DB: "/data/powerguardian.db"

volumes:
  powerguardian-data:
```

Open your browser and go to `http://<your-host-ip>:8181` to complete the setup wizard.

---

## Changing the Port

PowerGuardian runs on port **8181** by default. To use a different port (e.g. **9090**), change **two values** in the compose:

| What | Default | Example |
|------|---------|---------|
| `ports` | `"8181:8181"` | `"9090:9090"` |
| `PG_ADDR` | `"0.0.0.0:8181"` | `"0.0.0.0:9090"` |

Example for port 9090:

```yaml
services:
  powerguardian:
    image: ghcr.io/powerguardianos/powerguardian-docker:latest
    container_name: powerguardian
    restart: unless-stopped
    ports:
      - "9090:9090"
    volumes:
      - powerguardian-data:/data
    environment:
      PG_ADDR: "0.0.0.0:9090"
      PG_DB: "/data/powerguardian.db"

volumes:
  powerguardian-data:
```

> You can also change the port later from **Settings → General → HTTP Port** inside the dashboard. After saving, update your compose file and restart the container.

---

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `PG_ADDR` | `0.0.0.0:8181` | Listen address and port |
| `PG_DB` | `/data/powerguardian.db` | SQLite database path |
| `PG_VAULT_KEY` | *(auto-generated)* | AES-256 encryption key for the vault. Auto-generated on first start and stored in the database. Set this manually if you want to survive a database loss. |

---

## Supported Platforms

| Platform | Device |
|----------|--------|
| `linux/amd64` | x86-64 servers, PCs |
| `linux/arm64` | Raspberry Pi 4/5, NanoPi R3S |
| `linux/arm/v7` | Raspberry Pi 2/3, QNAP NAS, older ARM devices |

---

## Data Persistence

All data (database, vault, settings) is stored in the `powerguardian-data` Docker volume mounted at `/data`. This volume persists across container restarts and updates.

To back up your data, copy the volume contents or use the built-in backup feature in Settings → Backup.
