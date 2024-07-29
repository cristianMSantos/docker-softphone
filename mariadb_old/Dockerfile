# Use a imagem oficial do MariaDB como base
FROM mariadb:10.11

# Defina variáveis de ambiente para a senha do root e outras configurações
ENV MYSQL_ROOT_PASSWORD=asterisk
ENV MYSQL_DATABASE=asterisk
ENV MYSQL_USER=asterisk
ENV MYSQL_PASSWORD=asterisk

# Atualize o repositório e instale pacotes adicionais
RUN apt-get update && \
    apt-get install -y \
    unixodbc \
    unixodbc-dev \
    wget \
    tar \
    dpkg \
    odbcinst

# Copie o pacote .deb para o contêiner
COPY odbc-mariadb_3.1.15-3_amd64.deb /tmp/odbc-mariadb.deb

# Instale o pacote .deb e resolva dependências
RUN dpkg -i /tmp/odbc-mariadb.deb && \
    apt-get install -f -y && \
    rm /tmp/odbc-mariadb.deb

# Exponha a porta padrão do MariaDB
EXPOSE 3306
