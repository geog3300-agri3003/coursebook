# GEOG3300 and AGRI3003

## Dev environment

Copy docker image to `.devcontainer` directory.

```
cp ${PWD}/docker/Dockerfile ${PWD}/.devcontainer
```

## Docker 

Docker image for GEOG3300 and AGRI3003 at UWA. 

Build:

```
cd docker
```

```
docker build -t geog3003-agri3003-2024 .
```

Docker hub:

Login to docker hub:

```
docker login -u jmad1v07
```

Tag image with docker hub username:

```
docker tag geog3003-agri3003-2024 jmad1v07/geog3003-agri3003-2024
```

Push the image:

```
docker push jmad1v07/geog3003-agri3003-2024
```

## Run the container

```
docker run -it --rm \
  -p 8888:8888 \
  -v "/Users/00094708/Dropbox (Personal)/work/uwa/teaching/advanced-spatial-analysis-3300":/home/jovyan/work \
  jmad1v07/geog3003-agri3003-2024:latest
```
