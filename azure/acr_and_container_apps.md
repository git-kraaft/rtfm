# ACR and Container Apps

## ACR

List repository
```bash
az acr repository list --name <acr-name>
```

List tags:
```bash
az acr repository show-tags \
    --name mycompanyacr \
    --repository backend
```

Delete a specific image:
```bash
az acr repository delete \
    --name mycompanyacr \
    --image backend:1.0.0
```

## Container Apps
Create an image and push it to the registry:
```bash
docker build -t egk-local-test .
docker tag <local-image> <suggested-image> --> docker tag egk-local-test <acr-login-server>/egk-pdfad:2026-06.30.1
az acr login --name <registry-name>
docker push <suggested-image>
