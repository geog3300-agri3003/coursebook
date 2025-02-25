# GEOG3300 and AGRI3003

## Dev environment

Open the directory in the devcontainer to access the mkdocs tools for deploying the coursebook. 

Copy docker image to `.devcontainer` directory.

```
cp ${PWD}/docker/Dockerfile ${PWD}/.devcontainer
```

### Deploy the coursebook

The coursebook is deployed as a GitHub Project Pages site. Project Pages site files get deployed to a dedicated branch (gh-pages). Checkout the main branch and run: 

```
mkdocs gh-deploy
```

## Docker 

Docker image for GEOG3300 and AGRI3003 at UWA. 

### Single architecture builds:

Build:

```
cd docker
docker build --platform linux/amd64 -t jmad1v07/geog3300-agri3003-2024-amd64:latest . 
docker build --platform linux/arm64 -t jmad1v07/geog3300-agri3003-2024-arm64:latest . 
```

Docker hub:

Login to docker hub:

```
docker login -u jmad1v07
```

Push the image:

```
docker push jmad1v07/geog3300-agri3003-2024-amd64:latest
docker push jmad1v07/geog3300-agri3003-2024-arm64:latest

```

### Multi-architecture builds:

Switch to multi-architecture building context and driver

```
docker buildx create --use
```

Multi-architecture build without cache
```
docker buildx build --push --platform linux/amd64,linux/arm64 --no-cache -t jmad1v07/geog3300-agri3003-2024:latest . 
```

With cache
```
docker buildx build --push --platform linux/amd64,linux/arm64 -t jmad1v07/geog3300-agri3003-2024:latest . 
```


## Run the container

```
docker run -it --rm \
  -p 8888:8888 \
  -v "/Users/00094708/Dropbox (Personal)/work/uwa/teaching/advanced-spatial-analysis-3300":/home/jovyan/work \
  jmad1v07/geog3300-agri3003-2024:latest
```

```
docker run -it --rm \
  -p 8889:8888 \
  -v "/Users/00094708/Dropbox (Personal)/work/uwa/teaching/advanced-spatial-analysis-3300":/home/jovyan/work \
  jmad1v07/geog3300-agri3003-2024:latest
```
