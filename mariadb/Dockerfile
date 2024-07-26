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
    libtool-ltdl \
    libtool-ltdl-devel \
    mariadb-connector-odbc

# Exponha a porta padrão do MariaDB
EXPOSE 3306
