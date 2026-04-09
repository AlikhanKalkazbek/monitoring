# Monitoring stack

This repository runs a small observability stack with:

- Grafana
- Prometheus
- Tempo
- OpenTelemetry Collector
- Node Exporter

## Services

- Grafana: `http://SERVER_IP:3000`
- OTLP gRPC ingest: `SERVER_IP:4317`
- OTLP HTTP ingest: `SERVER_IP:4318`

Prometheus, Tempo, and Node Exporter stay internal to Docker by default.

## Server deployment

Run these commands on your Linux server:

```bash
git clone https://github.com/AlikhanKalkazbek/monitoring.git
cd monitoring
cp .env.example .env
```

Edit `.env` and set a strong Grafana password:

```bash
nano .env
```

Start the stack:

```bash
docker compose pull
docker compose up -d
```

Check status:

```bash
docker compose ps
docker compose logs -f --tail=100
```

Open Grafana at `http://SERVER_IP:3000` and sign in with the credentials from `.env`.

## Firewall

Open only the ports you need:

- `3000/tcp` for Grafana
- `4317/tcp` for OTLP gRPC if applications send traces over gRPC
- `4318/tcp` for OTLP HTTP if applications send traces over HTTP

Example with UFW:

```bash
sudo ufw allow 3000/tcp
sudo ufw allow 4317/tcp
sudo ufw allow 4318/tcp
sudo ufw enable
```

## Updating

```bash
git pull
docker compose pull
docker compose up -d
```

## Useful commands

```bash
docker compose logs -f grafana
docker compose logs -f prometheus
docker compose logs -f tempo
docker compose logs -f otel-collector
docker compose logs -f node-exporter
```
