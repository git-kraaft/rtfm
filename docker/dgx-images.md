On Apple Silicon, build a Linux ARM64 image with Buildx and push it directly to ACR. The Linux platform marker in pyproject.toml ensures the container gets CUDA PyTorch; your native macOS environment still gets regular macOS PyTorch.

## 1. Prerequisites on the Mac

Start Docker Desktop, then verify Buildx:

docker buildx version
docker buildx inspect --bootstrap

Authenticate with Azure and ACR:

az login
az acr login --name rwaicont

Your existing repository configuration identifies:

Registry:   rwaicont.azurecr.io
Repository: tp_pii_service

Azure recommends az acr login for interactive developer pushes. ACR authentication documentation (https://learn.microsoft.com/azure/container-registry/container-registry-get-started-docker-cli?tabs=azure-cli)

## 2. Build and push ARM64 directly

Use an immutable release tag:

PII_IMAGE_TAG=2026-09-23-arm64-1
PII_IMAGE_REF=rwaicont.azurecr.io/tp_pii_service:${PII_IMAGE_TAG}

Build for DGX Spark and push directly to ACR:

docker buildx build \
--platform linux/arm64 \
--tag "${PII_IMAGE_REF}" \
--push \
.

Do not use --load here. --push sends the resulting ARM64 image directly to ACR and avoids storing the large image in Docker Desktop.

Apple Silicon can build Linux ARM64 natively through Docker Desktop’s Linux VM; no x86 emulation is needed. Docker multi-platform builds (https://docs.docker.com/build/building/multi-platform/)

## 3. Verify the architecture in ACR

docker buildx imagetools inspect "${PII_IMAGE_REF}"

The output must contain:

Platform: linux/arm64

You can also check ACR:

az acr repository show-tags \
--name rwaicont \
--repository tp_pii_service \
--output table

## 4. Deploy on DGX Spark

Authenticate the DGX with ACR:

az login
az acr login --name rwaicont

Or use your production service principal, managed identity, or ACR token with docker login.

Pull the immutable tag:

PII_IMAGE_REF=rwaicont.azurecr.io/tp_pii_service:2026-09-23-arm64-1
docker pull "${PII_IMAGE_REF}"

Verify the image architecture:

docker image inspect "${PII_IMAGE_REF}" \
--format '{{.Os}}/{{.Architecture}}'

Expected:

linux/arm64

Run it:

docker run -d \
--name pii \
--gpus all \
--restart unless-stopped \
--env-file .env \
-p 8012:8012 \
"${PII_IMAGE_REF}"

Verify CUDA:

docker exec pii python -c \
"import torch; print(torch.__version__, torch.version.cuda, torch.cuda.is_available(), torch.cuda.get_device_name(0))"

Then check service health:

curl http://127.0.0.1:8012/api/v1/health-check

## Docker Compose alternative

On the DGX:

services:
pii:
image: rwaicont.azurecr.io/tp_pii_service:2026-09-23-arm64-1
platform: linux/arm64
gpus: all
restart: unless-stopped
env_file:
- .env
ports:
- "8012:8012"

Deploy:

docker compose pull
docker compose up -d
docker compose logs -f pii

One caveat: the existing Azure Pipeline uses an ubuntu-latest agent and does not explicitly select ARM64, so it will ordinarily publish an x86-64 image. Continue using the Mac Buildx command for DGX releases until the pipeline is updated for linux/arm64. ACR
supports both architecture-specific images and multi-architecture image indexes. ACR multi-architecture documentation (https://learn.microsoft.com/en-us/azure/container-registry/push-multi-architecture-images)
