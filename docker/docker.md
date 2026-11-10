# Docker Cheat Sheet

```bash
# Show running docker containers
docker ps
```
## Start Shell in Running Container

```bash
docker compose exec <service-name> sh 
```
## Images

```bash
docker image ls
```

## Volumes

```bash
# show unused volumes
docker volume ls -f dangling=true

# delete volumes
docker volume rm <volume name>
```

## Reload after docker-compose modification

```bash
# force-recreate
docker compose --env-file "deployments/${DEPLOYMENT_ENV}/compose.env" -f docker-compose.yml -f docker-compose.debug.yml up -d --no-deps --force-recreate [container name]
```

## Show Logs

```bash
docker compose --env-file "deployments/${DEPLOYMENT_ENV}/compose.env" logs -f docextract
```