# Docker Image Optimization Report

## Project Overview

Confronto tra due immagini Docker della stessa applicazione Flask:

- Dockerfile.naive
- Dockerfile.optimized

---

# 1. Build Commands

```bash
docker build -f Dockerfile.naive -t app:naive .
docker build -f Dockerfile.optimized -t app:optimized .
docker images | grep app
```

---

# 3. Image Size Comparison

| Image | Base Image | Size |
|---|---|---|
| app:naive | ubuntu:22.04 | 752MB |
| app:optimized | python:alpine | 53.6MB |

---

# 4. Size Reduction Calculation

Formula:

```text
((naive - optimized) / naive) × 100
```

Calculation:

```text
((752 - 53.6) / 752) × 100 = 92.87%
```

## Final Reduction

✅ Size reduction achieved: **92.9%**

---

# 5. Optimization Techniques Used

## 5.1 Multi-stage Build

### What it is

A multi-stage build uses multiple FROM instructions inside the same Dockerfile.

Example:

```dockerfile
FROM python:3.12 AS builder

# install dependencies

FROM python:3.12-alpine

# copy only required files
```

### Why it reduces image size

The first stage contains temporary build tools such as:

- compilers
- caches
- development libraries

Only the necessary application files are copied into the final image.

This avoids shipping unnecessary files in production.

### Benefits

- smaller images
- faster deployments
- better security

---

## 5.2 Alpine Linux

### What it is

Alpine Linux is a minimal Linux distribution designed for containers.

Example:

```dockerfile
FROM python:3.12-alpine
```

### Why it reduces image size

Ubuntu images contain many packages and utilities that are unnecessary for a simple Flask app.

Alpine includes only essential components.

### Benefits

- drastically smaller image size
- faster image download
- lower storage usage

---

## 5.3 .dockerignore

### What it is

The `.dockerignore` file excludes unnecessary files from the Docker build context.

Example:

```text
__pycache__/
.git/
venv/
*.log
```

### Why it reduces image size

Without `.dockerignore`, Docker may copy:

- git history
- virtual environments
- cache files
- logs

These files unnecessarily increase image size.

### Benefits

- smaller build context
- faster builds
- cleaner images

---

## 5.4 Layer Caching

### What it is

Docker stores each instruction as a layer.

Example:

```dockerfile
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
```

### Why it improves performance

If `requirements.txt` does not change, Docker reuses the cached dependency layer instead of reinstalling everything.

### Benefits

- faster rebuilds
- lower resource usage
- more efficient development workflow

---

# 6. Conclusion

The optimized Docker image successfully reduced the application size from:

- 752MB
to
- 53.6MB

This represents a reduction of approximately **92.9%**, which is significantly higher than the required 70%.

The main optimizations responsible for this improvement were:

- multi-stage builds
- Alpine Linux
- `.dockerignore`
- efficient Docker layer caching

The optimized image is more suitable for:

- cloud deployment
- CI/CD pipelines
- faster downloads
- reduced storage usage
- improved production efficiency
