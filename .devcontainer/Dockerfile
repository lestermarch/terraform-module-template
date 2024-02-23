# Use a base image with Azure Powershell
FROM mcr.microsoft.com/azure-powershell:ubuntu-22.04

# Silence some warnings and set the non-interactive mode for apt-get
ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies
RUN apt-get update && apt-get install -y \
  curl \
  fontconfig \
  git \
  gnupg2 \
  python3-pip \
  wget \
  unzip

# Install a Nerd Font (Fira Code as an example)
ENV FONT_NAME=CascadiaCode
ENV FONT_VERSION=2111.01
RUN wget https://github.com/microsoft/cascadia-code/releases/download/v${FONT_VERSION}/${FONT_NAME}-${FONT_VERSION}.zip -O ${FONT_NAME}.zip \
  && unzip ${FONT_NAME}.zip -d /usr/share/fonts \
  && rm ${FONT_NAME}.zip \
  && fc-cache -f -v

# Install Oh-My-Bash
RUN bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)" --unattended \
    && sed -i 's/OSH_THEME="font"/OSH_THEME="rr"/g' ~/.bashrc

# Install Terraform
ENV TERRAFORM_VERSION=1.7.4
RUN wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip -O terraform.zip \
    && unzip terraform.zip -d /usr/local/bin \
    && chmod +x /usr/local/bin/terraform \
    && rm terraform.zip \
    && terraform --version

# Install TFLint
ENV TFLINT_VERSION=0.50.3
RUN curl -sL https://github.com/terraform-linters/tflint/releases/download/v${TFLINT_VERSION}/tflint_linux_amd64.zip -o tflint.zip \
    && unzip tflint.zip -d /usr/bin \
    && rm tflint.zip

# Install terraform-docs
ENV TERRAFORM_DOCS_VERSION=0.17.0
RUN curl -sL https://github.com/terraform-docs/terraform-docs/releases/download/v${TERRAFORM_DOCS_VERSION}/terraform-docs-v${TERRAFORM_DOCS_VERSION}-linux-amd64.tar.gz | tar -xz -C /usr/bin

# Install Checkov
RUN pip install checkov

# Install pre-commit
RUN pip install pre-commit

# Install Azure CLI
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash

# Install GitHub CLI
ARG GITHUB_CLI_VERSION="2.44.1"
RUN wget https://github.com/cli/cli/releases/download/v${GITHUB_CLI_VERSION}/gh_${GITHUB_CLI_VERSION}_linux_amd64.tar.gz && \
    tar xvf gh_${GITHUB_CLI_VERSION}_linux_amd64.tar.gz && \
    cp gh_${GITHUB_CLI_VERSION}_linux_amd64/bin/gh /usr/local/bin/ && \
    rm -rf gh_${GITHUB_CLI_VERSION}_linux_amd64.tar.gz

# Install PowerShell Az module
RUN pwsh -Command "Install-Module -Name Az -AllowClobber -Scope AllUsers -Verbose -Force"

# Set working directory to /workspace as default
WORKDIR /workspace
CMD ["/bin/bash"]
