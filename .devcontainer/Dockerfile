# Base image — kept in sync with devcontainer.json's build.dockerfile reference.
FROM debian:sid

# Install only what's needed to bootstrap the dev user and shell at image-build
# time. Runtime/build deps are installed by the postCreateCommand scripts so
# they stay in sync with CI.
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends \
        sudo \
        zsh \
        git \
        curl \
        ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Create the 'dev' user with a home directory and zsh as the default shell.
# Grant passwordless sudo so the postCreateCommand scripts can use apt-get.
RUN useradd --create-home --shell /usr/bin/zsh dev \
    && echo "dev ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/dev \
    && chmod 0440 /etc/sudoers.d/dev

# Switch to the dev user for all subsequent instructions.
USER dev

# Set environment variables.
ENV CARGO_TARGET_DIR=/home/dev/cargo-output
ENV DISPLAY=host.docker.internal:0
ENV XDG_RUNTIME_DIR=/run/user/1000

# Set up the work directory to match the bind-mount target defined in
# devcontainer.json.
WORKDIR /workspace/warp

# Make scripts and cargo binaries accessible in the PATH for both bash and zsh.
RUN echo 'export PATH=/home/dev/setup/script:/home/dev/.cargo/bin:$PATH' >> /home/dev/.bashrc \
    && echo 'export PATH=/home/dev/setup/script:/home/dev/.cargo/bin:$PATH' >> /home/dev/.zshrc

# Ensure the SSH runtime directory exists with correct ownership (needed if
# clients connect via Remote-SSH extension rather than Dev Containers attach).
RUN mkdir -p /run/sshd && sudo chown -R dev:dev /run/sshd
