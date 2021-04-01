# Thinktecture Webinar: Small and secure Docker images for .NET

This repository contains all samples from the webinar held during March 2021.

## Iterative approach

During the webinar, we kept on optimizing the `Dockerfile` starting with the default `Dockerfile` produced by Visual Studio 2019 or Visual Studio for Mac.

## Build

To build a certain Docker image execute the following command:

```bash
docker build . -f Dockerfile -t webinar-api:0.0.1
```

Depending on the iteration, replace `Dockerfile` and `0.0.1` with the concrete value as shown below:

```bash
docker build . -f 2.Dockerfile -t webinar-api:0.0.2
```

## Verification

Each iteration should be verified based on several metrics such as

- Image Size
- Layer Size
- Vulnerabilities
- Sample API functionality

All those metrics can be verified using the following command:

```bash
# print all webinar images and their size
docker image ls --format "table {{.Repository}}{{.Tag}}\t{{.Size}}" | grep webinar

TAG="0.0.1"
LOCAL_PORT=8080
CONTAINER_PORT=80
# inspect layer sizes
docker history webinar-api:$TAG

# scan for vulnerabilities
docker scan webinar-api:$TAG

# verify that the api still works
$CONTAINER_ID=$(docker run -d -p $LOCAL_PORT:$CONTAINER_PORT webinar-api:$TAG)
docker logs $CONTAINER_ID
curl -ix GET http://localhost:$LOCAL_PORT/weatherforecast
docker rm -f $CONTAINER_ID
```
