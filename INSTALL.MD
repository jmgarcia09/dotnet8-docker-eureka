## Activar Docker en un Proyecto .NET 8

A continuación, se detallan los pasos para activar Docker en un proyecto .NET 8.

---

### Prerequisitos

Instalar docker (o docker desktop si tiene windows). Puede utilizar el siguiente enlace para seguir los pasos de instalacion.

[Pagina Docker Desktop](https://docs.docker.com/desktop/install/windows-install/)

**Nota**: Si se encuentra en Windows, se recomienda instalar WSL2 para poder utilizar Linux sobre el ambiente docker y simplificar su uso.

[Instalacion WSL2](https://learn.microsoft.com/en-us/windows/wsl/install)

Para validar que docker ya se encuentra habilitado dentro de tu sistema, ingresa al terminal y ejecuta:

```bash
docker ps -a
```

Este comando retorna los contenedores que se encuentran actualmente corriendo.

---

### Paso 1: Crear un Proyecto .NET 8

Primero, crea un nuevo proyecto .NET 8 ejecutando el siguiente comando en tu terminal, como por ejemplo:

```bash
dotnet new console -n MiProyecto
```

### Paso 2: Agregar Dockerfile

Crea un archivo llamado `Dockerfile` en el directorio raíz de tu proyecto con el siguiente contenido:

```shell
# Usa la imagen base de .NET 8
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app

# Copia los archivos necesarios y restaura las dependencias
COPY . .
RUN dotnet restore

# Genera la aplicación
FROM base AS build
WORKDIR /src
RUN dotnet build -c Release -o /app

# Publica la aplicación
FROM build AS publish
RUN dotnet publish -c Release -o /app

# Define la imagen final
FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "MiProyecto.dll"]
```

### Paso 3: Generar imagen de Proyecto

Al tener este archivo dentro del proyecto ya se podra generar una imagen que puede ser utilizada para ejecutar el proyecto dentro de docker.

Para generar la imagen es necesario ejecutar el siguiente comando:

```shell
docker build -t miproyecto:1.0.0 .
```

Esto iniciara la generacion de una imagen con la version que se indique (ej. 1.0.0), y luego podra ser visualizada con el siguiente comando:

```shell
docker image ls
```

### Paso 4: Ejecutar imagen creada

Luego de creada la imagen esta debe ser ejecutada en docker con el siguiente comando:

```shell
docker run -p 5689:8080 miproyecto:1.0.0
```

> Nota: Actualmente la imagen de net8 ejecuta por defecto el proyecto en el puerto 8080, por lo el `-p 5689:8080 `  hace referencia al puerto que sera expuesto del lado de tu maquina contra el del contenedor, como por ejemplo: `-p {puerto_de_maquina_fisica:puerto_contenedor}`
>
> Con esta indicacion, al correr el comando y levantar el contenedor, podremos encontrar nuestro servicio corriendo sobre el puerto 5689 de tu maquina (eg. localhost:5689)
