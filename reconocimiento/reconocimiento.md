# Fase 1 | Reconocimiento

## Objetivo

Identificar el entorno vulnerable entregado por TryHackMe y confirmar qué servicio PHP está expuesto dentro del laboratorio.

## Contexto conocido de la sala

La descripción del laboratorio plantea un servidor PHP vulnerable y una explotación realizada desde la sesión de acceso remoto del propio entorno. La referencia a `127.0.0.1:8088` debe verificarse en la instancia activa antes de documentarla como resultado.

## Procedimiento

> Pendiente de ejecución. Los siguientes datos se completarán con las salidas reales del laboratorio.

- [ ] Iniciar la máquina de TryHackMe.
- [ ] Establecer la conexión indicada por la plataforma.
- [ ] Identificar interfaces y servicios accesibles desde la máquina de laboratorio.
- [ ] Confirmar el puerto y proceso que atiende PHP.
- [ ] Obtener la versión de PHP y el SAPI, cuando sea posible.
- [ ] Registrar sistema operativo, servidor web y configuración relevante.

## Registro de resultados

| Elemento | Resultado |
|---|---|
| IP de laboratorio | Pendiente |
| Método de acceso | Pendiente |
| Puerto del servicio | Pendiente |
| Servidor web / SAPI | Pendiente |
| Versión PHP | Pendiente |
| Estado de la sesión | Pendiente |

## Evidencias

Guardar las capturas en `../evidencias/01-reconocimiento/`.

No registrar credenciales, tokens de acceso ni claves privadas en el repositorio.

## Análisis

La vulnerabilidad reside en el runtime de PHP y no necesariamente en el código de la aplicación. Por ello, revisar únicamente el framework o sus dependencias no permite descartar la exposición: hay que identificar la versión y el modo de ejecución de PHP.

## Conclusión

Pendiente de completar después de ejecutar el reconocimiento.