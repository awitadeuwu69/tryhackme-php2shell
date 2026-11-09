# Fase de explotación | Laboratorio TryHackMe

## Alcance

Esta sección documenta exclusivamente la máquina asignada por la sala PHP2Shell de TryHackMe.

## Preparación del objetivo

El contenedor identificado fue `php2shell-target`, con PHP 8.4.26 y el endpoint local:

```text
http://127.0.0.1:8088/index.php
```

El script de explotación se ejecutó desde el directorio proporcionado por el laboratorio.

## Comando ejecutado

```bash
python3 exploit.py -u http://127.0.0.1:8088/index.php
```

El exploit utilizó los offsets locales cargados desde:

```text
out/offsets.env
```

La salida observada mostró estos valores de offsets:

| Parámetro | Valor observado |
|---|---|
| `zif` | `0x465370` |
| `dtor` | `0x65eb60` |
| `htdata` | `0x690d8` |

Estos offsets pertenecen al entorno concreto del laboratorio y no deben reutilizarse como valores universales para otras compilaciones de PHP.

## Etapas observadas

1. **Heap leak:** el exploit obtuvo una dirección base del chunk de heap.
2. **Rotación de worker:** la salida indicó rotación del worker mediante HTTP.
3. **libphp leak:** se obtuvo una dirección base de la biblioteca PHP.
4. **Preparación de etapa final:** se mostraron las direcciones `HTDATA` y `ZIF`.
5. **Ejecución:** el exploit informó `W3 attempt 1` y devolvió la identidad del proceso.

## Prueba de ejecución

La salida final fue:

```text
PROOF: uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Esto confirma ejecución de comandos en el contexto del usuario `www-data` dentro del contenedor.

**Límite de la prueba:** no se obtuvo evidencia de ejecución como root ni de escape del contenedor. El resultado demostrado es ejecución de comandos dentro del entorno PHP vulnerable con los privilegios del proceso web.

La flag solicitada por TryHackMe fue validada durante la sesión y permitió completar la sala. Se omite su valor del repositorio público para no publicar la respuesta del ejercicio.

## Resultado de TryHackMe

La plataforma mostró **Room completed**, con 8 tareas completadas y 80 puntos obtenidos.

## Recomendaciones de evidencia

- Guardar capturas del reconocimiento y de la salida final.
- Mantener la flag fuera del repositorio público.
- No publicar cookies, credenciales, tokens ni datos de acceso.
- Si se comparten capturas, revisar que no contengan información sensible adicional.

## Conclusión

La cadena de explotación completó las etapas de fuga de memoria y alcanzó ejecución de comandos como `www-data`. La sala quedó completada. El resultado respalda la existencia de una ruta de ejecución remota en el entorno vulnerable preparado por TryHackMe.
