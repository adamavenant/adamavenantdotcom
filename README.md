# Adam Avenant's website

A static website served by an unprivileged Nginx container. The source is in `dist/`; there is no build step for the page itself.

## Run locally

Install Docker with the Compose plugin and start Docker, then run from this directory:

```sh
docker compose up --build -d
```

Open <http://localhost:8080>. After changing files in `dist/`, rebuild with the same command. To stop and remove the container:

```sh
docker compose down
```

Set `PORT=8090` before `docker compose up` to use a different host port. The default binding is `127.0.0.1`, so only the host can access it.

## Linux server

Install Docker and the Compose plugin, copy or clone this repository on the server, and run the same commands. To allow connections from other machines, explicitly bind to all interfaces:

```sh
BIND_ADDRESS=0.0.0.0 PORT=8080 docker compose up --build -d
```

The container serves plain HTTP on port 8080. Use a reverse proxy with TLS for public access and configure your firewall appropriately. The container restarts automatically unless explicitly stopped.

## Azure

The same image can run in Azure Container Apps. Build an image for `linux/amd64`, push it to a registry your Container App can access, and configure HTTP ingress with target port `8080`. Azure terminates HTTPS at ingress. An ARM Mac can build an image for Azure with Docker Buildx:

```sh
docker buildx build --platform linux/amd64 -t <registry>/<image>:<tag> --push .
```

Use a registry and Azure subscription you control; do not put credentials in the repository. See Microsoft's [Container Apps deployment guide](https://learn.microsoft.com/en-us/azure/container-apps/containerapp-up) for resource creation, registry access, and deployment options. No Azure resources or custom-domain settings are created by this repository.
