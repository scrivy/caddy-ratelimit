# caddy-ratelimit
A Caddy Docker image with the [`caddy-ratelimit`](https://github.com/mholt/caddy-ratelimit) HTTP rate limiting module compiled in, published to the GitHub Container Registry.

The image is built from the official Caddy builder image with `xcaddy`, then the resulting binary is copied into the stock `caddy:2` runtime image — so everything else about the official image (entrypoint, command, volumes, default config, CA certificates) is unchanged.

## Quick start

```bash
docker run -d --name caddy \
  -p 80:80 -p 443:443 -p 443:443/udp \
  -v "$PWD/caddy:/etc/caddy" \
  -v caddy_data:/data \
  -v caddy_config:/config \
  ghcr.io/scrivy/caddy-ratelimit:latest
```

The image ships the upstream default `Caddyfile` (a static file server for `/usr/share/caddy`), so mount your own config directory at `/etc/caddy` to do anything useful.

## Using the rate limiter

The module adds the `rate_limit` HTTP handler directive.

```caddyfile
example.com {
	rate_limit {
		zone dynamic {
			key    {remote_host}
			events 20
			window 1m
		}
	}

	reverse_proxy localhost:8080
}
```
