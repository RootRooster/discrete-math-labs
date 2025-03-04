FROM ubuntu:22.04

# Avoid interactive prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# Set timezone
RUN apt-get update && apt-get install -y tzdata \
    && ln -fs /usr/share/zoneinfo/Etc/UTC /etc/localtime \
    && dpkg-reconfigure --frontend noninteractive tzdata

# Install essential tools and dependencies
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    git \
    build-essential \
    python3-pip \
    python3-dev \
    python3-setuptools \
    python3-venv \
    sudo \
    vim \
    nano \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install SageMath
RUN apt-get update && apt-get install -y \
    sagemath \
    sagemath-jupyter \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Create a non-root user for better dev container experience
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Set up working directory
WORKDIR /workspace

# Make sure the user owns the workspace directory
RUN chown -R $USERNAME:$USERNAME /workspace

# Switch to the non-root user
USER $USERNAME

# Set up Python environment
RUN python3 -m pip install --user --upgrade pip \
    && python3 -m pip install --user jupyter

# Add sage to PATH
ENV PATH="/usr/bin:${PATH}"

# Default command
CMD ["bash"]
