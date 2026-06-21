# Elasticsearch + Kibana Stack

Docker Compose setup for a 3-node Elasticsearch cluster with Kibana.

## Stack

- Elasticsearch 7.2.0 (3 nodes: es01, es02, es03)
- Kibana

## Usage

```bash
docker-compose up -d
```

Elasticsearch will be available at `http://localhost:9200`.

## Files

- `docker-compose.yml` — Main stack definition
- `elasticsearch.yml` / `kibana.yml` — Service configuration
- `elasticsearch.compose` / `kibana.compose` — Config secrets
- `elastic-certificates.p12` — TLS certificate
- `shakespeare_6.0.json` — Sample dataset for testing
- `moloch` — Network traffic analysis tool config
