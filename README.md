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

| Python Version | Ansible Version | Docker Tags |
|---------------|----------------|-------------------------------------------------------------------|
| 3.12          | 2.15.13        | `python3.12-2.15.13-latest`, `python3.12-2.15.13`, `latest`       |
| 3.12          | 2.14.18        | `python3.12-2.14.18-latest`, `python3.12-2.14.18`                 |
| 3.11          | 2.13.13        | `python3.11-2.13.13-latest`, `python3.11-2.13.13`                 |
| 3.10          | 2.12.10        | `python3.10-2.12.10-latest`, `python3.10-2.12.10`                 |
| 3.9           | 2.11.12        | `python3.9-2.11.12-latest`, `python3.9-2.11.12`                   |

### Alternative Tags

- `latest` - Points to the latest Python/Ansible version (3.12 with Ansible 2.15)
- `python3.X-ansibleY.Z-latest` - Latest build for specific Python/Ansible combination
- `python3.X-ansibleY.Z` - Specific version tag

## Quick Start

1. **Pull an image:**

   ```bash
   docker pull ghcr.io/ginanck/molecule-runner:python3.10-2.12.10-latest
   ```

2. **Run Molecule tests:**

   ```bash
   docker run -it --rm -v $(pwd):/workspace -w /workspace ghcr.io/ginanck/molecule-runner:python3.10-2.12.10-latest molecule test
   ```

3. **Interactive shell:**

   ```bash
   docker run -it --rm ghcr.io/ginanck/molecule-runner:python3.10-2.12.10-latest /bin/bash
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

   ```bash
   # Prepare requirements file
   cat requirements-2.12.10.txt requirements-common.txt > dockerfiles/requirements.txt
   
   # Build the image
   docker build -f dockerfiles/Dockerfile.ansible2.12 -t molecule-runner:python3.10-2.12.10-latest dockerfiles/
   ```

### Testing Images

Test a built image with a simple Molecule command:

```bash
docker run --rm -v $(pwd):/workspace -w /workspace \
  ghcr.io/ginanck/molecule-runner:python3.10-2.12.10-latest \
  molecule --version
```

### GitHub Actions

The repository uses GitHub Actions to automatically build and push images to GitHub Container Registry on:

- **Push to master**: Builds and pushes all Python versions
- **Pull Requests**: Builds images for testing (no push)
- **Manual Trigger**: Workflow can be manually triggered with force push option

Images are tagged with:

- `python3.X-ansibleY.Z-latest` - Latest for that Python/Ansible combination
- `python3.X-ansibleY.Z` - Specific version for that combination
- `latest` - Latest overall (Python 3.12 with Ansible 2.15)

## License

MIT License - see [LICENSE](LICENSE) file for details.
