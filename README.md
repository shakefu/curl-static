# curl-static

Docker images containing a static build of curl.

## Tags

- [latest](https://github.com/shakefu/curl-static/blob/master/Dockerfile.glibc) - based on [busybox:glibc](https://hub.docker.com/_/busybox)

## Usage

This image can be used standalone to run `curl` or can be used as a base image
in a multistage build to include a statically compiled curl command in a glibc
environment. You must have glibc libraries because they are dynamically loaded
by libcurl's DNS resolver, and without them DNS will not work.

### Running from container

```bash
# Using curl to download to the host system
docker run shakefu/curl-static -sL 'https://github.com/shakefu/curl-static/archive/master.tgz' > curl-static.tgz
```

### Using in a multistage build

```docker
FROM shakefu/curl-static AS curl

# This stage used for curl binary

# This *must* be glibc otherwise DNS does not work
FROM busybox:glibc AS output

# Grab our static binary
COPY --from=curl /usr/local/bin/curl /usr/local/bin/curl

# Do other productive image things
```
