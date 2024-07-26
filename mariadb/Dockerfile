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
    unixodbc-dev

# Instale dependências necessárias
RUN apt-get update && \
    apt-get install -y wget tar dpkg && \
    wget https://downloads.mariadb.com/Connectors/odbc/connector-odbc-3.1.14/mariadb-connector-odbc-3.1.14-debian-buster-amd64.tar.gz && \
    tar -xzvf mariadb-connector-odbc-3.1.14-debian-buster-amd64.tar.gz && \
    cd mariadb-connector-odbc-3.1.14-debian-buster-amd64 && \
    apt-get install -f -y && \
    rm -rf mariadb-connector-odbc-3.1.14-debian-buster-amd64.tar.gz mariadb-connector-odbc-3.1.14-debian-buster-amd64

# Exponha a porta padrão do MariaDB
EXPOSE 3306
