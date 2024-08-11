# Use the latest slim version of Debian
FROM --platform=$TARGETPLATFORM debian:bookworm-slim

# Set ARG for platform-specific commands
ARG TARGETPLATFORM

# Update and install necessary packages
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    python3-venv \
    nodejs \
    npm \
    openssh-server \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Set up SSH
RUN mkdir /var/run/sshd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin prohibit-password/' /etc/ssh/sshd_config && \
    sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config && \
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config

# Add the SSH public key
RUN mkdir -p /root/.ssh && \
    echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDYmKRRcCFxo+HjBFfF23+mCu8YEQiQp/6qHCDvn+1SdpYO4Vw4OwHNoEAcMHTrlOiZWd4Wdg9t8pREKvDoGJ4x5bKwwOZbBMA9kE+VYUQDDSxLA+GKlX+EWqYQsh5arLoNwIgnU68wjMpJeAYuCiZIPpW2FCLHMm5tpvHUOUKK3sGGlog/ppieOHiWHI/VMKJgj9JnCAQNac0qTBJuQ3jgkKPfbSf6Ns70krHjyfncwmzfP7Cq8f3XRWOFfSMH9tNN0khnqbDOCfybULkdNwJLhIbkVaDJSQqmq15JbB5G451DFLgfVAA859EaG4d56x23bnm/B2aDJmOowWHWoo799GZUauJo65vf6i6YtLc3CXTk7UCMPVYuiR9PAoFeg582tk349zBmXFlJEg36qsdpIUN5Torb1OdRhyESAAyFdsQH5GzeYFJVtMzv/SX954eLuKUhOpQDkX4LIPz6MEdw6Kmuy4cShICS2fAyS/kuXTfzszY45mHMb2otSrlI6cqBWGG7u4aS6HmNZtAMPAhvCEVCEqTIujMPV6uk7ZJx/QMgGz3oeqcUqVH2zM/tt5tLAvHbuB+/HGXGZgDt0lXq68Y4fc0CIceIrciTkP6m7TV3zsw3j17Tnx4+KlxohsfaqnVHNA87MnKItFCO1ybLb+LzWQu5RnkZyl+K0Vpl/Q== wesleydean631@gmail.com" >> /root/.ssh/authorized_keys && \
    chmod 700 /root/.ssh && \
    chmod 600 /root/.ssh/authorized_keys

# Create and activate Python virtual environment
ENV VIRTUAL_ENV=/opt/venv
RUN python3 -m venv $VIRTUAL_ENV

# Copy initial .bashrc with virtual environment activation to a temporary location
COPY .bashrc /etc/skel/.bashrc

# Copy the script to ensure .bashrc is in the root directory
COPY initialize.sh /usr/local/bin/initialize.sh
RUN chmod +x /usr/local/bin/initialize.sh

# Ensure the virtual environment and pip setup
RUN $VIRTUAL_ENV/bin/pip install --upgrade pip

# Expose SSH port
EXPOSE 22

# Init .bashrc
CMD ["/usr/local/bin/initialize.sh"]

