# Docker Cheat Sheet

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
``
