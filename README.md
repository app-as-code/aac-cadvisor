<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/icon-dark.svg">
    <img src="assets/icon.svg" alt="cAdvisor logo" width="128">
  </picture>
</p>

<h1 align="center">aac-cadvisor</h1>

<p align="center">Self-hosted deployment definition for cAdvisor, a resource usage and performance analyzer for running containers.</p>

<p align="center">
  <a href="https://github.com/google/cadvisor"><img alt="Upstream" src="https://img.shields.io/badge/upstream-google%2Fcadvisor-blue?logo=github"></a>
  <img alt="Image" src="https://img.shields.io/badge/image-gcr.io%2Fcadvisor%2Fcadvisor%3Av0.55.1-2496ED?logo=docker&logoColor=white">
  <a href="https://github.com/google/cadvisor/blob/master/LICENSE"><img alt="License" src="https://img.shields.io/badge/license-Apache--2.0-green"></a>
</p>

## About

cAdvisor (Container Advisor) analyzes resource usage and performance characteristics of
running containers. It runs as a single container, reads the host's cgroup and Docker
data, and exposes per-container CPU, memory, filesystem and network statistics through a
built-in web interface and a Prometheus-compatible metrics endpoint.

Key features (as provided by upstream):

- Per-container resource usage and performance statistics
- Built-in web UI for live inspection of containers and the host
- Prometheus metrics endpoint for scraping by external monitoring systems
- Support for Docker containers and other container runtimes

## Upstream

- Project: <https://github.com/google/cadvisor>
- Container image: `gcr.io/cadvisor/cadvisor:v0.55.1`
- License: Apache-2.0 (upstream project)

## Quick start (Docker Compose)

The Compose file is rendered from `service.yml` by the App-as-Code template engine. This
service has no secrets and no external networks, so no `.env` file is required:

```bash
docker compose up -d
```

After startup the web interface is available on container port `8080`.

## Configuration

Environment variables (defaults from `service.yml`):

| Variable | Default | Description |
|---|---|---|
| `PUID` | `1000` | User ID passed to the container environment |
| `PGID` | `1000` | Group ID passed to the container environment |
| `TZ` | `Europe/Berlin` | Time zone |

Ports:

| Port | Protocol | Purpose |
|---|---|---|
| `8080` | TCP | cAdvisor web interface and Prometheus metrics (`web`) |

Volumes (all mounted read-only):

| Host path | Container path | Purpose |
|---|---|---|
| `/` | `/rootfs` | Host root filesystem for disk and system statistics |
| `/var/run` | `/var/run` | Docker socket and runtime state |
| `/sys` | `/sys` | Kernel cgroup and device information |
| `/var/lib/docker` | `/var/lib/docker` | Docker container metadata and layer information |

Runtime flags (from `service.yml`):

| Flag | Value | Effect |
|---|---|---|
| `--disable_metrics` | `percpu,sched,process,tcp,udp,advtcp,hugetlb,referenced_memory,resctrl,cpu_topology` | Disables metric groups to reduce the number of exported series |
| `--storage_duration` | `1m` | In-memory retention of statistics |
| `--housekeeping_interval` | `30s` | Interval for collecting container statistics |
| `--docker_only` | `true` | Only report Docker containers |
| `--store_container_labels` | `false` | Do not store all container labels as metric labels |
| `--whitelisted_container_labels` | `com.docker.compose.service,com.docker.compose.project` | The only container labels exported as metric labels |

## Deploy with App-as-Code

1. Edit `service.yml`. It is the single source of truth for image, ports, volumes and
   per-stage overrides. Do not edit the rendered output.
2. Pushing to `main` runs the service pipeline, which renders the manifests with
   `aac-template-engine` and triggers the deployment.
3. Releases of this definition are published under <https://github.com/app-as-code/aac-cadvisor>.

## Credits and license

cAdvisor is developed by the cAdvisor authors (Google) and licensed under Apache-2.0. This
repository only contains the deployment definition for it and is not a fork of the
application. Icon sources and licenses are documented in
[`assets/ICON_SOURCE.md`](assets/ICON_SOURCE.md).
