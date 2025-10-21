# Molecule Runner Docker Images

Lightweight Alpine Linux-based Docker images optimized for Ansible Molecule testing with pre-installed Ansible, Python, and testing tools.

## Table of Contents

- [Available Images](#available-images)
- [Quick Start](#quick-start)
- [Features](#features)
- [Local Development](#local-development)
- [License](#license)

## Available Images

All images are based on Alpine Linux for minimal size and maximum efficiency.

| Python Version | Base Image | Docker Pull Command |
|----------------|------------|---------------------|
| 3.12 | Alpine 3.18 | `docker pull ghcr.io/ginanck/molecule-runner:python3.12` |
| 3.11 | Alpine 3.18 | `docker pull ghcr.io/ginanck/molecule-runner:python3.11` |
| 3.10 | Alpine 3.18 | `docker pull ghcr.io/ginanck/molecule-runner:python3.10` |
| 3.9  | Alpine 3.18 | `docker pull ghcr.io/ginanck/molecule-runner:python3.9` |
| 3.8  | Alpine 3.18 | `docker pull ghcr.io/ginanck/molecule-runner:python3.8` |

### Alternative Tags

- `latest` - Points to the latest Python version (3.12)
- `python3.X-latest` - Latest build for specific Python version
- `python3.X-YYYYMMDD-HHmmss` - Timestamped builds for reproducibility

## Quick Start

1. **Pull an image:**

   ```bash
   docker pull ghcr.io/ginanck/molecule-runner:python3.10
   ```

2. **Run Molecule tests:**

   ```bash
   docker run -it --rm -v $(pwd):/workspace -w /workspace ghcr.io/ginanck/molecule-runner:python3.10 molecule test
   ```

3. **Interactive shell:**

   ```bash
   docker run -it --rm ghcr.io/ginanck/molecule-runner:python3.10 /bin/bash
   ```

## Features

Each image includes:

- **Ansible Ecosystem**: ansible-core, ansible-lint, ansible-compat
- **Molecule Testing**: molecule, molecule-docker, molecule-podman
- **Python Tools**: flake8, cryptography, PyYAML
- **System Tools**: Docker CLI, Git, SSH client, Bash
- **Build Tools**: GCC, musl-dev, libffi-dev (for package compilation)
- **Collections**: Pre-installed community.docker collection
- **User Setup**: Dedicated `ansible` user (UID 1000) with proper permissions

## Local Development

### Building Images Locally

1. **Build a specific Python version:**

   ```bash
   # Prepare requirements file
   cat requirements-3.10.txt requirements-common.txt > dockerfiles/requirements.txt
   
   # Build the image
   docker build -f dockerfiles/Dockerfile.python3.10 -t molecule-runner:python3.10 dockerfiles/
   ```

2. **Build all images:**

   ```bash
   # Build script for all Python versions
   for version in 3.8 3.9 3.10 3.11 3.12; do
     echo "Building Python $version image..."
     cat requirements-$version.txt requirements-common.txt > dockerfiles/requirements.txt
     docker build -f dockerfiles/Dockerfile.python$version \
       -t ghcr.io/ginanck/molecule-runner:python$version \
       dockerfiles/
   done
   ```

### Testing Images

Test a built image with a simple Molecule command:

```bash
docker run --rm -v $(pwd):/workspace -w /workspace \
  ghcr.io/ginanck/molecule-runner:python3.10 \
  molecule --version
```

### GitHub Actions

The repository uses GitHub Actions to automatically build and push images to GitHub Container Registry on:

- **Push to master**: Builds and pushes all Python versions
- **Pull Requests**: Builds images for testing (no push)
- **Manual Trigger**: Workflow can be manually triggered with force push option

Images are tagged with:

- `python3.X` - Latest for that Python version
- `python3.X-latest` - Alias for latest
- `python3.X-YYYYMMDD-HHmmss` - Timestamped builds

## License

MIT License - see [LICENSE](LICENSE) file for details.
