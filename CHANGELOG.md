# Changelog

All notable changes to this deployment definition are documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

## [0.0.1]

### Added

- Initial App-as-Code service definition for cAdvisor v0.55.1.
- Web interface and metrics endpoint on container port 8080.
- Read-only host mounts for root filesystem, `/var/run`, `/sys` and `/var/lib/docker`.
- Metric export limited to Docker containers with a reduced set of metric groups.
- README with configuration reference and project icon.
