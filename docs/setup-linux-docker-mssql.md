# Instalacion mssql con Docker linux

Esta guia muestra como configurar la imagen oficial de [mssql](https://hub.docker.com/r/microsoft/mssql-server/)  de microsoft usando Docker en un SO Ubuntu Linux

## Prerequisitos

1. Instalar docker [Docker](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) en la VPS

Set up del repositorio de Docker.

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Para instalar la última version usa el siguiente comando:

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-compose
```

Verifica que la instalacion sea correcta ejecutando un `hello-world`

```bash
sudo docker run hello-world
```

2. Instalar [bun](curl -fsSL <https://bun.sh/install> | bash) para poder ejecutar el script de javascript para subir los backups de la bd a AWS S3

```bash
curl -fsSL https://bun.sh/install | bash
```

3. (Opcional) Instalar [Neovim](https://github.com/neovim/neovim/blob/master/INSTALL.md#linux) un editor de texto bastante completo dentro de la terminal

```bash
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz
sudo rm -rf /opt/nvim
sudo tar -C /opt -xzf nvim-linux-x86_64.tar.gz

# Then add this to your shell config (~/.bashrc, ~/.zshrc, ...):
export PATH="$PATH:/opt/nvim-linux-x86_64/bin"
```

## Configuración

Necesitamos crear un archivo docker compose para levantar un contenedor
de mssql oficial de Microsoft, y a su vez mapear una ruta dentro del
contenedor una ruta dentro de nuestro VPS de linux

    NOTAS
    - La ruta donde se generan los backups dentro del contenedor es `/var/opt/mssql/data/`
    - El puerto por defecto usado por mssql es 1433

comando ejemplo para levantar una imagen de mssql usando la version mas reciente:

```bash
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=SUPER_PASSWORD" -e "MSSQL_PID=Express" -p 1433:1433  --name NOMBRE_DEL_CONTENEDOR -d mcr.microsoft.com/mssql/server:latest
```

    NOTAS
    El password debe de tener una mayúscula, minuscula, numero y un simbolo, y ser de 8 caracteres

Ejemplo de docker compose para levantar la BD:

```yaml
version: '3'

services:
  mssql:
    image: mcr.microsoft.com/mssql/server:2022-latest
    user: root
    container_name: yoox_test
    environment:
      - ACCEPT_EULA=Y
      - MSSQL_SA_PASSWORD=Marzo2025?
      - MSSQL_PID=Express
    ports:
      - "1433:1433"
    volumes:
      - ~/db_backups:/var/opt/mssql/data
    restart: unless-stopped

```
