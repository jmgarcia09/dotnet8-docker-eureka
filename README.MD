# Proyecto .NET 8 con Integración Eureka

Este proyecto es una aplicación de ejemplo que utiliza .NET 8 y está integrada con Eureka para la gestión de servicios y el descubrimiento de servicios en un entorno de microservicios.

## Requisitos Previos

Asegúrese de tener instalado lo siguiente antes de ejecutar la aplicación:

- [.NET SDK 8](https://dotnet.microsoft.com/download/dotnet/8.0)
- Un servidor Eureka en ejecución. Puede instalar Eureka Server utilizando [Spring Cloud Netflix](https://spring.io/projects/spring-cloud-netflix), o utilizar el siguiente comando de docker para iniciar un servidor listo para su uso
  ```shell
  docker run --publish 8761:8761 steeltoeoss/eureka-server
  ```

## Configuración

Antes de ejecutar la aplicación, asegúrese de configurar correctamente el archivo `appsettings.json` con la ubicación de su servidor Eureka:

```json
{
  "Eureka": {
    "ServiceUrl": "http://localhost:8761/eureka/" // Cambie la URL según la configuración de su servidor Eureka
  }
}
```

## Ejecución

1. Clone este repositorio en su máquina local.
2. Navegue hasta el directorio del proyecto.
3. Compile el proyecto utilizando el comando `dotnet build`.
4. Ejecute la aplicación utilizando el comando `dotnet run`

La aplicación se registrará automáticamente en el servidor Eureka especificado en la configuración y estará disponible para su descubrimiento por otros servicios.


### Creacion de imagen Docker

Si desea generar una imagen con docker, revisa el archivo [Generar Imagen Docker](INSTALL.MD)
