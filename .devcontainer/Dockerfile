# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.191.1/containers/debian/.devcontainer/base.Dockerfile

# [Choice] Debian version: bullseye, buster, stretch
ARG VARIANT="buster"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

ARG USERNAME=vscode

RUN SNIPPET="export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" \
    && mkdir /commandhistory \
    && touch /commandhistory/.bash_history \
    && chown -R $USERNAME /commandhistory \
    && echo "$SNIPPET" >> "/home/$USERNAME/.bashrc"

RUN sudo apt -y install lsb-release apt-transport-https ca-certificates \
    && sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
    && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends php8.0 php8.0-bcmath php8.0-bz2 php8.0-intl php8.0-gd php8.0-mbstring php8.0-mysql php8.0-zip php8.0-dom php8.0-yaml php8.0-curl mariadb-server apache2 nodejs npm

# Xdebug.
RUN apt-get install php8.0-xdebug

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install terminus.
RUN mkdir /utilities && composer create-project pantheon-systems/terminus /utilities/terminus --ignore-platform-req=ext-pcov \
    && echo -e '\nPATH="/utilities/terminus/bin:$PATH"' >> /home/vscode/.bashrc

# Install node and yarn
RUN npm install --global yarn

# Configure Apache.
RUN echo "Listen 80" >> /etc/apache2/ports.conf && \
    a2enmod rewrite
