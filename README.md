# KGMCloud — Coming Soon

A "Coming Soon" landing page built with **Vue 3**, **Vite**, and **Tailwind CSS v4**.

## Development

```bash
npm install
npm run dev
```

## Build

```bash
npm run build
npm run preview
```

## Docker

```bash
docker build -t kgmcloud-place .
docker run -p 8080:80 kgmcloud-place
```

## Deployment (Cosmos Cloud)

This repository includes a `Dockerfile` and `docker-compose.yml` ready for Cosmos Cloud.
Point Cosmos Cloud at this GitHub repository (or the built image) and it will build
and serve the static site behind its reverse proxy.
