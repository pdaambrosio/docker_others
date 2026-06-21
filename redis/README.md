# Redis

Redis and Sentinel configuration for Docker.

## Usage

```bash
# Run Redis container
docker run -itd --rm --name rd01 --network elk-net -p 6379:6379 redis:5.0

# Enable AOF persistence
docker exec -it rd01 redis-server --appendonly yes
```

## Files

- `redis.conf` — Redis configuration file
- `sentinel.conf` — Redis Sentinel configuration for high availability
- `dokcer_run` — Docker run command reference
- `data/` — Persistent data directory
