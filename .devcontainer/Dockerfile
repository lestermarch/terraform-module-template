# Use a base image with Azure Powershell
FROM mcr.microsoft.com/azure-powershell:ubuntu-22.04

# Install system dependencies
RUN apt-get update && apt-get install -y \
  curl \
  fontconfig \
  git \
  wget \
  unzip

# Install a Nerd Font (Fira Code as an example)
ENV FONT_NAME=CascadiaMono
ENV FONT_VERSION=3.1.1
RUN wget -P ~/.local/share/fonts https://github.com/ryanoasis/nerd-fonts/releases/download/v${FONT_VERSION}/${FONT_NAME}.zip \
  && cd ~/.local/share/fonts \
  && unzip ${FONT_NAME}.zip \
  && rm ${FONT_NAME}.zip \
  && fc-cache -fv

# Install Oh My Posh
RUN wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/posh-linux-amd64 -O /usr/local/bin/oh-my-posh && \
    chmod +x /usr/local/bin/oh-my-posh

# Get the default Oh My Posh theme
RUN mkdir -p /root/.poshthemes && \
    curl -o /root/.poshthemes/zash.omp.json https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/zash.omp.json

# Set the Oh My Posh theme in the PowerShell profile
ENV POSH_THEME_PATH="/root/.poshthemes"
ENV POSH_THEME="zash"
RUN mkdir -p ${POSH_THEME_PATH} \
    && curl -o ${POSH_THEME_PATH}/${POSH_THEME}.omp.json https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/${POSH_THEME}.omp.json
COPY Microsoft.PowerShell_profile.ps1 /home/${USERNAME}/.config/powershell/Microsoft.PowerShell_profile.ps1

# Install Terraform
ENV TERRAFORM_VERSION=1.7.4
RUN curl -fsSL https://apt.releases.hashicorp.com/gpg | apt-key add - \
    && apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main" \
    && apt-get update && apt-get install terraform=${TERRAFORM_VERSION}

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

# Install PowerShell Az module
RUN pwsh -Command "Install-Module -Name Az -AllowClobber -Scope AllUsers -Force"
