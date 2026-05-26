<h1 align="center">Docker Repository Template</h1>

<p align="center">
  <em>A GitHub repository template with Docker, pre-commit hooks, and CI/CD</em>
</p>

<p align="center">
  <a href="https://github.com/Tom-Notch/Docker-Repository-Template/actions/workflows/pre-commit.yml"><img src="https://github.com/Tom-Notch/Docker-Repository-Template/actions/workflows/pre-commit.yml/badge.svg" alt="pre-commit"></a>
  <img src="https://img.shields.io/badge/Docker-Required-2496ed?logo=docker&logoColor=white" alt="Docker">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT">
</p>

A GitHub repository template that uses Docker for easy deployment and testing, with pre-commit hooks and GitHub Actions CI/CD included.

> **TLDR:** Search for `todo` and update all occurrences to your desired name.

## Dependencies

- [Docker](https://docs.docker.com/get-docker/)

## Usage

### Base Repository

1. Change [LICENSE](LICENSE) if necessary
1. Modify [.pre-commit-config.yaml](.pre-commit-config.yaml) according to your needs
1. Modify/add GitHub workflow status badges in [README.md](README.md)

### Docker Config

1. Fill in all `todo-*` placeholders directly in [.env.example](.env.example) and commit — these are project-level constants, not secrets

   | Placeholder | Description |
   |-------------|-------------|
   | `todo-docker-user` | Your Docker Hub account username |
   | `todo-base-image` | Base image the Dockerfile builds from (e.g. `nvidia/cuda:13.0.0-cudnn-devel-ubuntu24.04`) |
   | `todo-image-name` | Name of the image you are building |
   | `todo-image-user` | Default user inside the image, used to determine the home folder |

1. Copy [.env.example](.env.example) to `.env` and add any user-specific secrets or local overrides:

   ```shell
   cp .env.example .env
   ```

   > `.env` is gitignored and will NOT be committed — it is the right place for secrets and per-user values. It is loaded automatically by docker compose.

1. Modify the service name from `todo-service-name` to your service name in [docker-compose.yml](docker-compose.yml), add additional volume mounting options such as dataset directories

1. Update [Dockerfile](docker/latest/Dockerfile) and [.dockerignore](.dockerignore) — the existing Dockerfile includes screen & tmux config, oh-my-zsh, cmake, and other basic tools

1. Run scripts to build, test, and push:

   | Script | Action |
   |--------|--------|
   | [build.sh](scripts/build.sh) | Build and test the image locally (uses `buildx` for multi-arch) |
   | [run_container.sh](scripts/run_container.sh) | Run and test a built image (`docker compose up -d` also works) |
   | [push.sh](scripts/push.sh) | Push the multi-arch image to Docker Hub |

   > The service mounts the entire repository onto `CODE_FOLDER` inside the container — modifications inside are reflected outside, useful for VS Code remote development.

## Developer Quick Start

```shell
bash scripts/dev_setup.sh
```

## Notes

- Supports `amd64` and `arm64` Docker images. To add other architectures, modify [build.sh](scripts/build.sh) and [docker-compose.yml](docker-compose.yml).
- The build stage has no GPU access. If your dependencies require GPU, build them inside a running container and commit to the final image.
