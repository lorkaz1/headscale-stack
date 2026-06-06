# Headscale Stack
l
## Voraussetzungen

- Docker oder Podman
- Domain
- DNS A-Record auf den Server

## Installation

Repository klonen:

```bash
git clone https://github.com/USERNAME/headscale-stack.git
cd headscale-stack
```

Dateien erstellen:

```bash
cp .env.example .env
cp config.yaml.example config.yaml
```

Domain in beiden Dateien anpassen.

Stack starten:

```bash
podman compose up -d
```

oder

```bash
docker compose up -d
```

Logs prüfen:

```bash
podman logs -f headscale
```

Ersten Benutzer anlegen:

```bash
podman exec -it headscale \
headscale users create admin
```

PreAuth Key erzeugen:

```bash
podman exec -it headscale \
headscale preauthkeys create \
-u admin \
--reusable
```

Nodes anzeigen:

```bash
podman exec -it headscale \
headscale nodes list
```