# gr4-ci

Base CI builder images for GNU Radio 4 and related repositories.

The repository is organized by base distro first, then by buildchain profile.
Each distro gets a base image that installs common prerequisites, and each
profile image builds on that base for a specific toolchain configuration.

## Layout

```text
builders/
  <distro>/
    base/
      Dockerfile
    profiles/
      <profile>/
        Dockerfile
```

## Conventions

- `builders/<distro>/base` contains the distro-wide prerequisite layer.
- `builders/<distro>/profiles/<profile>` contains toolchain-specific builder images.
- Profile Dockerfiles should `FROM` the matching distro base image.
- Add new distros by creating a sibling directory under `builders/`.
- Add new profiles by creating a sibling directory under the matching `profiles/` tree.

## Make Targets

Targets are generated from the directory tree.

```bash
make list
make build-<distro>-base
make build-<distro>-<profile>
```

## GitHub Actions

The manual workflow in [`.github/workflows/build-images.yml`](/home/josh/gnuradio/gr4-ci/.github/workflows/build-images.yml) discovers the builder tree at runtime, builds each distro base in its own job, then builds each profile in its own job and pushes the resulting images to GHCR.
