# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.245.2/containers/rust/.devcontainer/base.Dockerfile

# [Choice] Debian OS version (use bookworm on local arm64/Apple Silicon): buster, bullseye, bookworm
ARG VARIANT
FROM mcr.microsoft.com/vscode/devcontainers/rust:${VARIANT}

# Install additional packages
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends protobuf-compiler libprotobuf-dev libclang-dev

RUN apt install -y libgmp3-dev

COPY rust-toolchain.toml .

# Install cargo-binstall
RUN curl -L --proto '=https' --tlsv1.2 -sSf https://raw.githubusercontent.com/cargo-bins/cargo-binstall/main/install-from-binstall-release.sh | bash

RUN rustup toolchain install $(cat rust-toolchain.toml | grep channel | cut -d\" -f2) && \
    rustup default $(cat rust-toolchain.toml | grep channel | cut -d\" -f2) && \
    rustup component add clippy && \
    rustup component add rustfmt

RUN cargo binstall cargo-nextest --secure -y

# Install dojoup and scarb for vscode user
USER vscode
RUN curl -L https://install.dojoengine.org | bash
RUN curl --proto '=https' --tlsv1.2 -sSf https://docs.swmansion.com/scarb/install.sh | bash
ENV PATH=${PATH}:/workspaces/dojo/target/release:/home/vscode/.dojo/bin
RUN /home/vscode/.dojo/bin/dojoup
