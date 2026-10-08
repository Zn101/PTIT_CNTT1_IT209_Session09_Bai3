# Session 09 - Exercise 3: Package a static HTML page with a Dockerfile

## Goal

Write a minimal Dockerfile that packages a static HTML page into a Docker image based on Nginx.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The static page: `<h1>Hello Docker Session 09!</h1>` |
| `Dockerfile` | Builds the image from `nginx:alpine` and copies the page in |
| `README.md` | This document |

## Dockerfile explained

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

- `FROM nginx:alpine` - uses the official Nginx image on Alpine Linux as the base. It is small and already configured to serve static files on port 80.
- `COPY index.html /usr/share/nginx/html/index.html` - overwrites Nginx's default welcome page with our own `index.html`. `/usr/share/nginx/html/` is the default web root of the Nginx image.

No `CMD` or `EXPOSE` is needed: both are inherited from the base image, which already starts Nginx in the foreground and exposes port 80.

## How to run

Run these commands from the folder containing the `Dockerfile`:

```bash
# 1. Build the image
docker build -t my-html-app:v1 .

# 2. Start a container, mapping host port 8081 to container port 80
docker run -d -p 8081:80 --name html-app my-html-app:v1

# 3. Test
curl http://localhost:8081
```

## Expected result

```
<h1>Hello Docker Session 09!</h1>
```

## Useful checks

```bash
docker images my-html-app     # confirm the image exists with tag v1
docker ps                     # confirm html-app is running with 0.0.0.0:8081->80/tcp
docker logs html-app          # view the Nginx access log
```

## Cleanup

```bash
docker stop html-app
docker rm html-app
docker rmi my-html-app:v1
```
