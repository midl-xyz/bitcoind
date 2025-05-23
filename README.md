# Bitcoin Core Dockerized

This repository provides a Dockerized setup for running Bitcoin Core, including building the binaries from source, verifying their integrity, and running the Bitcoin daemon (`bitcoind`) in a containerized environment.

## Features

- **Build from Source**: The Dockerfile builds Bitcoin Core binaries from source, ensuring a transparent and customizable build process.
- **Binary Verification**: Includes a Python script (`verify-29.0.py`) to verify the integrity and authenticity of Bitcoin Core binaries using GPG signatures and SHA256 checksums.
- **Customizable User and Group IDs**: The `docker-entrypoint.sh` script allows setting custom `UID` and `GID` for the `bitcoin` user inside the container.
- **Multi-Stage Build**: The Dockerfile uses a multi-stage build process to keep the final image lightweight.
- **Preconfigured Data Directory**: The container is preconfigured to use `/home/bitcoin/.bitcoin` as the data directory.
- **Exposed Ports**: The container exposes the default Bitcoin Core ports for P2P and RPC communication.

## Getting Started

### Prerequisites

- Docker installed on your system.
- AWS credentials (if publishing the Docker image to Amazon ECR).

### Build the Docker Image

To build the Docker image locally:

```bash
docker build -t bitcoin-core:29.0 .
```

### Run the Container

To run the Bitcoin daemon:

```bash
docker run -d \
  --name bitcoin-core \
  -v /path/to/bitcoin-data:/home/bitcoin/.bitcoin \
  -p 8332:8332 -p 8333:8333 \
  bitcoin-core:29.0
```

### Verify Binaries

The `verify-29.0.py` script can be used to verify the integrity of Bitcoin Core binaries. For example:

```bash
python3 verify-29.0.py pub 29.0
```

## GitHub Actions Workflow

The repository includes a GitHub Actions workflow (`.github/workflows/build.yaml`) to automate the build and publish process:

- Builds the Docker image using `docker/build-push-action`.
- Publishes the image to Amazon ECR.

To trigger the workflow, use the `workflow_dispatch` event in GitHub Actions.

## Environment Variables

The following environment variables can be used to customize the container:

- `UID`: User ID for the `bitcoin` user inside the container.
- `GID`: Group ID for the `bitcoin` group inside the container.
- `BITCOIN_DATA`: Path to the Bitcoin data directory (default: `/home/bitcoin/.bitcoin`).

## Exposed Ports

- `8332`: RPC port.
- `8333`: P2P port.
- `18332`: Testnet RPC port.
- `18333`: Testnet P2P port.
- `18444`: Regtest P2P port.

## License

This project is licensed under the MIT License. See the [LICENSE](https://opensource.org/licenses/MIT) file for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## Acknowledgments

- [Bitcoin Core](https://bitcoincore.org) for the source code and binaries.
- [Docker](https://www.docker.com) for containerization.
- [GitHub Actions](https://github.com/features/actions) for CI/CD automation.