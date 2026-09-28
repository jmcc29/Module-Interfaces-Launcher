## LEVANTAR PROYECTOS CON DOCKER COMPOSE

## PASOS PARA LEVANTAR EL PROYECTO

1. Clonar el repositorio forkeado e ingresar a la misma

```
cd Module-Interfaces-Launcher
```

2. Crear un .env basado en el .env.example y llenar las variables de entorno correspondientes

```
cp .env.example .env
```

3. Ejecutar el siguiente comando para reconstruir los sub-modulos

```
git submodule update --init --recursive
```

## CONFIGURACIÓN DE LOS FRONTENDS

4. En desarrollo con el Compose del launcher, la configuración compartida se toma del `.env` raíz y se inyecta explícitamente en cada contenedor. No es necesario crear un `.env` dentro de cada submódulo para este modo.

Las claves de herramienta son fijas en Compose:

- Hub: `hub`
- Beneficiary: `beneficiary`
- Sales: `sales`
- Collections: `collections`

`GATEWAY_INTERNAL_URL` es una dirección server-to-server. `HUB_PUBLIC_ORIGIN` y los orígenes públicos de herramientas deben ser alcanzables por el navegador. No se deben usar secretos en variables `NEXT_PUBLIC_*`.

Para ejecutar una interfaz de forma independiente, copiar su propia plantilla y ajustar los valores:

```sh
cd Beneficiary-Interface
cp .env.example .env
```

Repetir solamente para la interfaz que se ejecutará fuera del Compose.

## DESPUÉS DE CONFIGURAR TODAS LOS .ENVS LEVANTAR CON DOCKER PARA VERSIÓN DEV - DESARROLLO

5. Comando para construir las imágenes y levantar los contenedores

```
docker compose build --no-cache && docker compose up
```

## PRODUCCIÓN

Crear un archivo separado para producción:

```sh
cp .env.production.template .env.production
```

Antes de construir:

- usar orígenes públicos HTTPS;
- establecer `AUTH_COOKIE_SECURE=true`;
- configurar `GATEWAY_INTERNAL_URL` con una URL HTTPS alcanzable desde los contenedores;
- verificar que los cuatro orígenes públicos coincidan con DNS o proxy;
- no reutilizar el `.env` de desarrollo.

Las variables `NEXT_PUBLIC_*` se entregan como argumentos de build porque Next.js las incorpora al bundle. Las variables de autenticación y destinos del Hub se entregan solamente al runtime del servidor.

Validar primero la interpolación sin levantar servicios:

```sh
docker compose --env-file .env.production -f docker-compose.prod.yml config --quiet
```

Después construir y levantar:

```sh
docker compose --env-file .env.production -f docker-compose.prod.yml build --no-cache
docker compose --env-file .env.production -f docker-compose.prod.yml up -d
```

## ////////////////////////////////////

## PARA EMPEZAR A COLABORAR

Cuando ya se tiene el los sub módulos se debe ingresar a cada uno y realizar lo siguiente

```
cd Beneficiary-Interface
```

```
checkout main
```

```
git remote add origin https://github.com/{nombre_colaborador}/Beneficiary-Interface.git
git remote add upstream https://github.com/MUTUAL-DE-SERVICIOS-AL-POLICIA/Beneficiary-Interface.git
```

Para redirigir a la rama principal `cd ..`

```
cd Login-Hub-Interface
```

```
checkout main
```

```
git remote add origin https://github.com/{nombre_colaborador}/Login-Hub-Interface.git
git remote add upstream https://github.com/MUTUAL-DE-SERVICIOS-AL-POLICIA/Login-Hub-Interface.git
```

Y PODRÁN REALIZAR LOS PULL REQUEST COMO SE HACE REGULARMENTE DE LOCAL -> REPOSITORIO PERSONAL -> REPOSITORIO PRINCIPAL

## /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

## CUANDO SE CREA UN NUEVO PROYECTO FRONTEND Y SE REQUIERA AÑADIR AL REPOSITORIO PRINCIPAL COMO SUB MÓDULO REALIZAR LO SIGUIENTE

## Pasos para añadir/crear los Git Submodules (un nuevo frontend)

1. Copiar el URL del nuevo repositorio y añadir en el clonado del repositorio padre MUSERPOL-MS local ejecutar:

```
git submodule add <enlace_del_nuevo_repositorio> <nombre_de_carpeta>
```

2. Añadir los cambios al repositorio MUSERPOL-MF (git add, git commit, git push) Ej:

git add .
git commit -m "Add submodule"
git push

3. Realizar un pull request

## ///////////////////////////////////////

INSTALACIÓN INDEPENDIENTE

Para levantar los proyectos de forma independiente sin docker compose
revisar los README de cada sub módulo ubicados en su repositorio.
