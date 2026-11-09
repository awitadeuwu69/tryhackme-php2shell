# Fase 1 | Reconocimiento

## Objetivo

Identificar el entorno vulnerable entregado por TryHackMe y confirmar qué servicio PHP está expuesto dentro del laboratorio.

## Entorno observado

| Elemento | Resultado |
|---|---|
| Usuario de la sesión remota | `ubuntu` |
| Contenedor | `php2shell-target` |
| Imagen | `php:8.4.26-apache` |
| Versión PHP comprobada | `PHP 8.4.26 (cli)` |
| Publicación de puertos Docker | `127.0.0.1:8088 -> 80/tcp` |
| Endpoint | `http://127.0.0.1:8088/index.php` |
| Respuesta HTTP | `OK` |

## Procedimiento ejecutado

### 1. Enumeración de contenedores

```bash
docker ps
```

Se identificó el contenedor `php2shell-target`, basado en `php:8.4.26-apache`, con el puerto 80 del contenedor publicado únicamente en la interfaz loopback del host de laboratorio, puerto 8088.

### 2. Versión de PHP

```bash
docker exec php2shell-target php -v
```

Resultado observado: PHP 8.4.26 (cli), Zend Engine 4.4.26 y OPcache 8.4.26.

La consulta se ejecutó mediante CLI dentro del contenedor. La imagen Apache indica el entorno web, pero esta salida por sí sola no identifica todos los detalles del SAPI que atiende cada petición.

### 3. Comprobación HTTP

```bash
curl -s http://127.0.0.1:8088/index.php
```

Respuesta observada:

```text
OK
```

### 4. Archivos del material de laboratorio

Se inspeccionó el directorio `~/php2shell-bypass-aslr-exp`, que contenía documentación, el script `exploit.py`, el directorio `out/`, registros y herramientas auxiliares.

## Análisis

El puerto 8088 no estaba expuesto como servicio público en la configuración observada: Docker lo enlazó a `127.0.0.1`. Por eso, el exploit se ejecutó desde la propia sesión remota de TryHackMe contra el loopback de esa máquina.

La versión PHP 8.4.26 observada es anterior a la versión corregida 8.4.27 indicada por el aviso de seguridad de PHP. Esta comparación es coherente con el objetivo del laboratorio, aunque la confirmación definitiva de exposición depende también de la configuración y del código de la cadena vulnerable.

## Conclusión

Se identificó el contenedor objetivo, se confirmó PHP 8.4.26 y se verificó que el endpoint local respondía. El reconocimiento permitió establecer el objetivo correcto para las siguientes pruebas.

## Evidencias

Las capturas originales de la sesión se conservan como evidencia del reconocimiento y la ejecución. No incluir en el repositorio público información sensible de acceso.
