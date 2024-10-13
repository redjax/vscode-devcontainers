# Base image layer: Setting the container image and creating the user
ARG CONTAINER_IMG_VARIANT
FROM python:${CONTAINER_IMG_VARIANT} AS base

# Create a non-root user named '{{ cookiecutter.container_user }}'
RUN useradd -m {{ cookiecutter.container_user }}

# Install necessary packages
FROM base AS os_setup
RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    curl \
    wget \
    iputils-ping \
    ## Run apt cleanup
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
FROM os_setup AS python_setup{% if cookiecutter.include_requirements_txt %}
COPY requirements.txt ./
RUN [ -f requirements.txt ] && pip install --no-cache-dir -r requirements.txt || echo "requirements.txt not found, skipping pip install."{% endif %}

# Runtime layer
FROM python_setup AS runtime
WORKDIR /workspace
USER {{ cookiecutter.container_user }}
