# Use a imagem base Debian mais recente
FROM debian:latest

# Atualize e instale pacotes necessários
RUN apt-get update && \
    apt-get install -y \
    nano \
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
    odbcinst \
    wget \
    tar \
    dpkg \
    mariadb-server \
    mariadb-client \
    && rm -rf /var/lib/apt/lists/*

# Baixe e extraia o Asterisk
RUN curl -sL http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-20.9.1.tar.gz | tar xz -C /usr/src

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

# Ajuste as permissões do diretório
RUN chown -R asterisk:asterisk /var/lib/asterisk /etc/asterisk /var/log/asterisk /var/spool/asterisk

# Copie arquivos de configuração do mariadb
COPY ./odbc/odbc.ini /etc/odbc.ini
COPY ./odbc/odbcinst.ini /etc/odbcinst.ini

# Defina variáveis de ambiente para a senha do root e outras configurações
ENV MYSQL_ROOT_PASSWORD=asterisk
ENV MYSQL_DATABASE=asterisk
ENV MYSQL_USER=asterisk
ENV MYSQL_PASSWORD=asterisk

# Copie o pacote .deb para o contêiner
COPY odbc-mariadb_3.1.15-3_amd64.deb /tmp/odbc-mariadb.deb

# Instale o pacote .deb e resolva dependências
RUN dpkg -i /tmp/odbc-mariadb.deb && \
    apt-get install -f -y && \
    rm /tmp/odbc-mariadb.deb

# Certifique-se de que o diretório do socket do MySQL existe
RUN mkdir -p /run/mysqld && chown mysql:mysql /run/mysqld

# Para dar acesso ao banco de dados para fora do container
COPY ./mariadb/50-server.cnf /etc/mysql/mariadb.conf.d/50-server.cnf

# Copie o script de entrada
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Exponha a porta padrão do MariaDB
EXPOSE 3306

# Use o script de entrada
ENTRYPOINT ["/entrypoint.sh"]
