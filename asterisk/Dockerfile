# Use a imagem base Debian mais recente
FROM debian:latest

# Atualize e instale o curl, gnupg2 e outros pacotes necessários
RUN apt-get update && \
    apt-get install -y \
    nano \
    systemctl \
    curl \
    gnupg2 \
    build-essential \
    libxml2-dev \
    libncurses5-dev \
    libnewt-dev \
    libsqlite3-dev \
    uuid-dev \
    libjansson-dev \
    libssl-dev \
    libedit-dev \
    libcurl4-openssl-dev \
    pkg-config \
    unixodbc \
    unixodbc-dev \
    libtool \
    libltdl-dev \
    odbcinst \
    && rm -rf /var/lib/apt/lists/* \
    && curl -sL http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-20.9.1.tar.gz | tar xz -C /usr/src

# Defina o diretório de trabalho
WORKDIR /usr/src/asterisk-20.9.1

# Configure, compile e instale o Asterisk
RUN ./configure && \
    make && \
    make install && \
    make samples && \
    make config && \
    ldconfig && \
    mkdir /etc/asterisk/keys && \
    ls -l /etc/asterisk/keys

# Crie o usuário e grupo 'asterisk'
RUN groupadd -r asterisk && useradd -r -g asterisk asterisk

# Defina o diretório de trabalho para o Asterisk
WORKDIR /var/lib/asterisk

# Copie arquivos de configuração do mariadb
COPY ./odbc/odbc.ini /etc/odbc.ini
COPY ./odbc/odbcinst.ini /etc/odbcinst.ini

# Ajuste as permissões do diretório
RUN chown -R asterisk:asterisk /var/lib/asterisk /etc/asterisk /var/log/asterisk /var/spool/asterisk

# Comando padrão para iniciar o Asterisk como o usuário 'asterisk'
CMD ["asterisk", "-f", "-U", "asterisk"]
