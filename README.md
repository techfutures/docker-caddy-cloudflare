# Caddy + Cloudflare DNS module
Uses the the latest official Caddy docker image and adds the Cloudflare DNS module so DNS-01 ACME challenge can be used. 

New packages are auto-scheduled to be built everyday at midnight UTC. 

Based on [Caddy Forum post how-to guide](https://caddy.community/t/how-to-guide-caddy-v2-cloudflare-dns-01-via-docker/8007).

## Example docker run command
```
docker run -d \
    --name caddy \
    --restart always \
    -p 80:80 \
    -p 443:443 \
    -v /etc/caddy/Caddyfile://etc/caddy/Caddyfile \
    -v /etc/caddy/.certs:/data \
    ghcr.io/techfutures/docker-caddy-cloudflare:main
```

## Example Caddyfile
```
hostname.example.com {
  reverse_proxy 127.0.0.1:8000
  tls {
     dns cloudflare APITOKENHERE
  }
}
```
