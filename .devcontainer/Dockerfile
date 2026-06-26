# Run commands essential for setting up the development environment
USER dev

# Set environment variables
ENV CARGO_TARGET_DIR=/home/dev/cargo-output
ENV DISPLAY=host.docker.internal:0
ENV XDG_RUNTIME_DIR=/run/user/dev

# Set up work directory
WORKDIR /workspace/warp

# Make dependencies executables accessible
RUN echo 'export PATH=/home/dev/setup/script:/home/dev/.cargo/bin:$PATH' >> /home/dev/.bashrc
RUN echo 'export PATH=/home/dev/setup/script:/home/dev/.cargo/bin:$PATH' >> /home/dev/.zshrc

# Configure git
RUN git config --global user.name "openhands" && git config --global user.email "openhands@all-hands.dev"

# Allow the container to be accessed via SSH
RUN mkdir -p /run/sshd && chown -R dev:dev /run/sshd

# Open port 22 for SSH connections
EXPOSE 22

# Commit to the current work directory
COPY . /workspace/warp

# Run user commands after container starts
CMD ["/usr/sbin/sshd", "-D"]
