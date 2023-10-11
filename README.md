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
## Example json config file for cert management only
```
{
  "apps": {
    "tls": {
      "certificates": {
        "automate": [
          "hostname.example.com"
        ]
      },
      "automation": {
        "policies": [ {
            "issuers": [ {
                "module": "acme",
                "challenges": {
                  "dns": {
                    "provider": {
                      "name": "cloudflare",
                      "api_token": "APITOKENHERE"
                    }
                  }
                }
              }
            ]
          }
        ]
      }
    }
  }
}
```
## Example docker compose
```
version: '3.7'
services:
  caddy:
    container_name: caddy
    restart: always
    image: "ghcr.io/techfutures/docker-caddy-cloudflare:main"
    volumes:
      - /etc/caddy/config.json://etc/caddy/config.json
      - /etc/caddy/.certs:/data
    command: ["caddy", "run", "--config", "/etc/caddy/config.json"]
```
