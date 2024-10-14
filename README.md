## Homeserver

A simple setup for my small homeserver that I run on a Raspberry Pi 5 using docker-compose and Traefik.

### Update a service

```
docker-compose pull <APP-NAME>
docker-compose up -d --force-recreate --no-deps <APP-NAME>
```

### Adding a new service

1. Create a new docker compose service in `apps/new-app/docker-compose.yml`.

```
services:
  new-app:
    image: new-app-image
    container_name: new-app
```

2. Add the service to `docker-compose.yml` in root:

```
new-app:
  extends:
    file: apps/new-app/docker-compose.yml
    service: new-app
  labels:
    - "traefik.enable=true"
    - "traefik.http.routers.startPage.rule=Host(`new-app.${BASE_DOMAIN}`)"
    - "traefik.http.routers.startPage.entrypoints=https"
    - "traefik.http.routers.startPage.tls=true"
    - "traefik.http.services.startPage.loadbalancer.server.port=80"
```

3. Add a new CNAME record in PiHole pointing to the Raspberry Pi:

```
new-app.base-domain.com <raspberry pi DNS A record>
```
