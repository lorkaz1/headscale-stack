# Headscale + Caddy

Minimaler Headscale-Stack mit automatischen Let's-Encrypt-Zertifikaten über Caddy.

## Features

* Headscale
* Caddy Reverse Proxy
* Automatische Let's-Encrypt-Zertifikate
* SQLite-Datenbank
* Podman- und Docker-kompatibel
* Keine zusätzliche UI
* Geringer Ressourcenverbrauch

## Voraussetzungen

* Linux-Server
* Docker oder Podman
* Eigene Domain
* DNS A-Record auf den Server

## DNS konfigurieren

Erstelle einen A-Record:

```text
headscale.example.com -> SERVER_IP
```

Beispiel:

```text
headscale.meinedomain.de -> 203.0.113.10
```

## Verzeichnisstruktur

```text
headscale/
├── compose.yml
├── Caddyfile
├── config/
│   └── config.yaml
└── data/
```
Data-Ordner erstellen
```text
mkdir data
```

## Konfiguration

### Domain anpassen

In `config/config.yaml`:

```yaml
server_url: https://headscale.example.com
```

In `Caddyfile`:

```caddy
headscale.example.com {
    reverse_proxy headscale:8080
}
```

## Stack starten

Docker:

```bash
docker compose up -d
```

Podman:

```bash
podman compose up -d
```

Logs anzeigen:

```bash
podman logs -f headscale
```

oder

```bash
docker logs -f headscale
```

## Benutzer anlegen

```bash
podman exec -it headscale \
headscale users create admin
```

## PreAuth-Key erstellen

```bash
podman exec -it headscale \
headscale preauthkeys create \
--user admin \
--reusable
```

Die Ausgabe enthält den Auth-Key für neue Geräte.

## Gerät verbinden

### Linux

```bash
tailscale up \
  --login-server https://headscale.example.com \
  --auth-key AUTH_KEY
```

### Windows

```powershell
tailscale.exe up `
  --login-server https://headscale.example.com `
  --auth-key AUTH_KEY
```

### Android / iOS

1. Tailscale-App öffnen
2. Mehrfach auf das Tailscale-Logo tippen
3. „Custom Control Server“ auswählen
4. Headscale-URL eintragen

## Nützliche Befehle

### Benutzer anzeigen

```bash
podman exec -it headscale headscale users list
```

### Geräte anzeigen

```bash
podman exec -it headscale headscale nodes list
```

### PreAuth-Keys anzeigen

```bash
podman exec -it headscale headscale preauthkeys list
```

### Logs anzeigen

```bash
podman logs -f headscale
```

## Update

Image aktualisieren:

```bash
podman compose pull
podman compose up -d
```

oder

```bash
docker compose pull
docker compose up -d
```

## Backup

Wichtige Daten:

```text
data/
config/config.yaml
```

Diese Verzeichnisse sollten regelmäßig gesichert werden.

## Ressourcenbedarf

Typischer Verbrauch:

* RAM: ca. 50–150 MB
* CPU: vernachlässigbar
* Speicherplatz: wenige MB

Geeignet für Raspberry Pi, Mini-PCs, VPS und Home-Server.
