# Configuración de Odoo y PostgreSQL en Docker

Este archivo YAML proporciona una configuración de Docker para ejecutar Odoo (versión 14.0) junto con PostgreSQL (versión 14.0) en contenedores separados. La configuración incluye volúmenes para persistir los datos de Odoo y PostgreSQL.

## Requisitos

Asegúrate de tener instalado Docker y Docker Compose en tu sistema antes de ejecutar esta configuración. Si no tienes Docker Compose instalado, sigue los siguientes pasos para instalarlo:

1. Abre una terminal o línea de comandos.
2. Ejecuta el siguiente comando para descargar la versión más reciente de Docker Compose:

```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

3. Aplica permisos de ejecución al archivo binario de Docker Compose:

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

4. Verifica que la instalación se haya realizado correctamente ejecutando el siguiente comando:

```bash
docker-compose --version
```

Deberías ver la versión de Docker Compose instalada en tu sistema.

## Uso

1. Crea un archivo llamado `docker-compose.yml` en tu directorio de proyecto.
2. Copia y pega el contenido del archivo YAML proporcionado en el archivo `docker-compose.yml`.
3. Opcionalmente, puedes modificar los puertos mapeados en la sección `ports` si deseas utilizar otros puertos en tu máquina host para acceder a Odoo.
4. Ejecuta el siguiente comando en tu terminal para iniciar los contenedores:

```bash
docker-compose up -d
```

Esto iniciará los contenedores de Odoo y PostgreSQL en segundo plano.

5. Para detener los contenedores, ejecuta el siguiente comando:

```bash
docker-compose down
```

Esto detendrá y eliminará los contenedores, pero conservará los volúmenes para mantener los datos persistentes.

## Configuración

El archivo YAML contiene dos servicios:

### Servicio Odoo

- Imagen: odoo:14.0
- Nombre del contenedor: odoo-dev
- Reinicio: a menos que se detenga explícitamente
- Dependencias: el servicio depende del servicio de base de datos (db)
- Puertos: el puerto 8069 del contenedor se mapea al puerto 8200 de la máquina host para acceder a Odoo
- Volúmenes: los datos de Odoo se almacenan en el volumen denominado `odoo-data`. La configuración personalizada se monta desde el directorio `config` en el contenedor. Los addons personalizados se montan desde el directorio `addons` en el contenedor.

### Servicio de base de datos (db)

- Imagen: postgres:14.0
- Nombre del contenedor: odoo-db-dev
- Reinicio: a menos que se detenga explícitamente
- Variables de entorno:
  - POSTGRES_DB: nombre de la base de datos (postgres en este caso)
  - POSTGRES_PASSWORD: contraseña de la base de datos (odoo en este caso)
  - POSTGRES_USER: nombre de usuario de la base de datos (odoo en este caso)
  - PGDATA: directorio de datos de PostgreSQL en el contenedor
  - Volúmenes: los datos de PostgreSQL se almacenan en el volumen denominado `db-data`.
