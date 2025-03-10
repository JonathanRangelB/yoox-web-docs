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

### Instalación de [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Para poder ejecutar el script de javascript para subir los backups de la bd a un bucket S3

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

### Instalación utilerias

instala (si es que aun no las tienenes) las utilerias que necesitaremos mas adelante en esta guia

```bash
apt install zip xclip -y
```

### Instalación (Opcional) [Neovim](https://github.com/neovim/neovim/blob/master/INSTALL.md#linux)

Un editor de texto bastante completo dentro de la terminal

```bash
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz
sudo rm -rf /opt/nvim
sudo tar -C /opt -xzf nvim-linux-x86_64.tar.gz

# Then add this to your shell config (~/.bashrc, ~/.zshrc, ...):
export PATH="$PATH:/opt/nvim-linux-x86_64/bin"
```

## Configuración docker-compose

Necesitamos crear un archivo docker compose para levantar un contenedor
de mssql oficial de Microsoft, y a su vez mapear una ruta dentro del
contenedor una ruta dentro de nuestro VPS de linux

    NOTAS
    - La ruta donde se generan los backups dentro del contenedor es `/var/opt/mssql/data/`
    - El puerto por defecto usado por mssql es 1433

`NO EJECUTAR`, es solo ilustrativo -> comando ejemplo para levantar una imagen de mssql usando la version mas reciente:

```bash
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=SUPER_PASSWORD" -e "MSSQL_PID=Express" -p 1433:1433  --name NOMBRE_DEL_CONTENEDOR -d mcr.microsoft.com/mssql/server:latest
```

    NOTAS
    El password debe de tener una mayúscula, minuscula, numero y un simbolo, y ser de 8 caracteres

Pasamos el comando anterior a un docker-compose.yml, nos quedaria algo asi:

```yaml
version: '3'

services:
  mssql:
    image: mcr.microsoft.com/mssql/server:latest
    user: root
    container_name: NOMBRE_DEL_CONTENEDOR
    environment:
      - ACCEPT_EULA=Y
      - MSSQL_SA_PASSWORD=SUPER_PASSWORD
      - MSSQL_PID=Express
    ports:
      - "1433:1433"
    volumes:
      - ~/db_backups:/var/opt/mssql/data
    restart: unless-stopped

```

El nombre del contenedor deberia de incluir el nombre del stage al cual pertenecera, algo como por ejemplo: `aplicacion_production`, `aplicacion_development` ó `aplicacion_staging`

Se agrego el campo user para que tenga permisos de poder ejecutar comandos, caso contrario arrojara un error y tendremos que eliminar el contenedor generado porque no nos servira.

## Backup DB bash script

Ahora necesitamos crear un archivo en nuestra ruta $HOME (preferentemente) con cualquier nombre, en este caso lo llamare `backup_db.sh` y le agregamos el siguiente contenido:

```bash
#!/bin/bash
FECHA=$(date +%Y%m%d_%H%M%S)
docker exec -it yoox_test /opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P 'PASSWORD' -C -Q "BACKUP DATABASE NOMBRE_BD TO DISK = '/var/opt/mssql/data/respaldo_$FECHA.bak' WITH FORMAT"
echo "Respaldo guardado en /db_backups/respaldo_$FECHA.bak"
zip ~/db_backups/respaldo_$FECHA.zip ~/db_backups/respaldo_$FECHA.bak
aws s3 mv ~/db_backups/respaldo_$FECHA.zip s3://respaldos-db-yoox
echo "Respaldo guardado en AWS S3!"
```

Cambiar solo en la sección del comando de docker el valor de `-P` por el password de tu db y el valor de `-Q` para que tenga el nombre de tu base de datos.

El script anterior guarda el archivo .bak en la ruta `/var/opt/mssql/data/` la cual teniamos previamente mapeada como `volumen` a la ruta de nuestro host llamado `~/db_backups`, comprimira el archivo .bak en un archivo standard .zip para posteriormente eliminar el archivo .bak y asi ahorrar espacio en la VPS

## Crontab para ejecutar periodicamente script

Antes que nada es buena idea validar que el crontab este habilitado en nuestro ubuntu linux con el siguiente comando:

```bash
sudo systemctl enable cron
sudo systemctl start cron
```

### Abrir crontab

Aquí es donde agregamos nuestros comandos que queremos ejecutar cada cierto tiempo, y lo ejecutamos con el comando:

```bash
crontab -e
```

Al abrirlo por primera vez nos preguntara por algun editor de texto, elegir el que mas sepas usar, si no sabes cual elegir te recomiendo usar `nano`

Veras un texto explicando el crontab, solo necesitamos agregar una linea al final del todo, dejemos todo el texto sin modificaciones, nos ayuda a manera de documentacion, podremos agregar la siguiente linea al final:

```bash
0 2 * * * ~/backup_db.sh >> ~/backup_db.log 2>&1
```

Lo cual se traduce a lo siguiente:

    Minuto  Hora  DíaMes  Mes  DíaSemana  Comando
    0       2     *       *    *          /ruta/completa/a/tu/script.sh

Esta instrucion ejecutara el script indicado todos los dias a las 2 de la madrugada

la parte que esta despues del script.sh `>> ~/backup_db.log 2>&1` nos generara un log cada vez que el script se ejecute colocando la nueva informacion al final del archivo backup.log, si el archivo no existe lo creara por nosotros, guarda tanto los logs que se mandan al stdinput como al stderror.

### Verificar nuestro cron job

Fácil, ejecutamos la siguiente linea:

```bash
crontab -l
```

esto deberia de mostrarnos la linea que agregamos
