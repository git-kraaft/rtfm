# Docker Cheat Sheet

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