# Remediación, detección y respuesta

## 1. Prioridad máxima: actualizar PHP

La actualización es la única corrección completa para CVE-2026-109618 y CVE-2026-109619.

| Rama | Primera versión corregida |
|---|---:|
| PHP 8.5 | 8.5.12 |
| PHP 8.4 | 8.4.27 |
| PHP 8.3 | 8.3.36 |
| PHP 8.2 | 8.2.35 |

Los proveedores de Linux pueden publicar paquetes con parches retroportados sin que el número de versión upstream coincida exactamente. Verificar el boletín del proveedor y el estado del paquete instalado. PHP 8.1 y anteriores están fuera del ciclo ordinario de soporte. PHP publicó backports de código fuente para las ramas 8.1, 8.0 y 7.4, y algunos proveedores pueden ofrecer paquetes corregidos. PHP 8.2 llega al fin de vida el 31 de diciembre de 2026; para instalaciones nuevas o planes de actualización, conviene priorizar PHP 8.3 o posterior. Para 7.3 y anteriores, confirmar con el proveedor si existe un backport; si no, actualizar a una rama soportada. Un backport corrige estas CVE, pero no devuelve soporte general a una rama EOL.

Tras actualizar, reiniciar Apache, PHP-FPM, php-cgi o todos los workers pertinentes. Comprobar la versión efectiva del runtime que atiende tráfico, no solo la versión de PHP CLI.

## 2. Mitigaciones temporales

Estas medidas reducen rutas concretas. No hacen que una instalación sin parche sea segura.

### CVE-2026-109618: OOM

- PHP recomienda configurar `memory_limit=-1` en un nivel administrativo que la aplicación no pueda sobrescribir, por ejemplo mediante `php_admin_value` en el pool FPM o la configuración Apache. Comprobar que ningún pool, directorio o aplicación vuelve a imponer un límite finito.
- Establecer `post_max_size` conservador: PHP sugiere 8 MB cuando no se necesitan cargas y 32 MB cuando sí son necesarias.
- Mantener `upload_max_filesize` por debajo de `post_max_size`.
- Limitar también en proxy/servidor el tamaño del cuerpo ya decodificado, cantidad de campos, partes multipart, cabeceras y profundidad de anidamiento. Los controles deben cubrir solicitudes aunque no exista un `Content-Length` utilizable.
- Usar límites de contenedor/cgroup como frontera de disponibilidad, idealmente con una política que termine solo el worker afectado.

**No usar `RLIMIT_AS` ni `RLIMIT_DATA` como sustitutos:** PHP advierte que pueden causar fallos de asignación que activen la ruta vulnerable. `memory_limit=-1` también tiene implicaciones de consumo de recursos, por lo que debe combinarse con aislamiento y límites externos apropiados.

### CVE-2026-109619: estado obsoleto

- Configurar `session.upload_progress.enabled=0` en `php.ini`, configuración del pool FPM o configuración administrativa del servidor, antes de que PHP procese la petición.
- Reiniciar todos los workers.
- No confiar en `ini_set()` ni en `.user.ini` para esta mitigación temprana.
- En PHP 8.5, `fatal_error_backtraces=0` bloquea una ruta confirmada de backtrace fatal, no todas las variantes.
- Eliminar configuraciones inválidas o obsoletas aplicadas por petición que generen diagnósticos antes de completar la inicialización.
- Corregir errores fatales y timeouts alcanzables remotamente, incluidos los que ocurren en callbacks, procesamiento de uploads/importaciones y limpieza.
- Si la arquitectura y el rendimiento lo permiten, considerar `pm.max_requests=1` en PHP-FPM para reciclar el worker tras cada petición. Es una reducción parcial del riesgo, no una corrección general ni sustituto del parche.

## 3. Detección

La detección más fiable no depende de reconocer una cadena exacta del exploit. La oportunidad está en el comportamiento posterior a la explotación: un proceso web que inicia intérpretes, shells, herramientas de red o modifica archivos de la aplicación.

### Telemetría a recopilar

- Árbol de procesos y proceso padre: Apache, PHP-FPM, php-cgi o equivalente.
- Ejecución de shells e intérpretes no habituales como `sh`, `bash`, `dash`, `python`, `perl`, `ruby` o `node`.
- Procesos de descarga o transferencia como `curl` y `wget` iniciados por workers web.
- Escritura o modificación de archivos PHP en directorios web, temporales o rutas de upload.
- Conexiones salientes inesperadas desde procesos web.
- Errores de PHP, OOM, fallos de segmentación, reinicios de workers y errores HTTP 5xx correlacionados temporalmente con peticiones POST.

### Indicadores de investigación

1. Correlacionar access log y error log por timestamp, IP origen, URI y código HTTP.
2. Revisar si el worker afectado terminó por segmentation fault o fue reiniciado.
3. Inspeccionar el árbol de procesos y los comandos ejecutados por el usuario del servicio web.
4. Buscar archivos nuevos o modificados en el webroot, directorios de upload y temporales.
5. Revisar conexiones salientes, tareas programadas, servicios y mecanismos de persistencia.
6. Si hay indicios de ejecución, tratar el sistema como comprometido: preservar logs y evidencia, aislarlo, rotar secretos accesibles desde el proceso y reconstruir desde una imagen confiable tras erradicar la persistencia.

Un fallo de segmentación puede ser un indicio de intento fallido o de explotación, pero no demuestra por sí solo compromiso. La ausencia de esos eventos tampoco descarta una explotación exitosa.

## 4. Ejemplos de reglas de detección

Las siguientes son ideas de lógica de detección para un EDR/SIEM, no reglas listas para producción. Ajustar nombres de procesos, rutas y falsos positivos al entorno.

| Regla | Condición de alerta | Severidad sugerida |
|---|---|---|
| Web worker spawns shell | Proceso PHP/Apache crea `sh`, `bash`, `dash` o equivalente. | Alta |
| Web worker launches network tool | Proceso web crea `curl`, `wget`, `nc` o `socat`. | Alta |
| Web root modification | Proceso web crea o modifica scripts ejecutables en webroot o upload. | Alta |
| PHP worker crash burst | Varios workers PHP/Apache terminan con señal de segmentation fault en una ventana corta. | Media/Alta |
| Unexpected egress | Worker web inicia conexiones a destinos externos no habituales. | Alta |

No bloquear automáticamente todo proceso hijo: algunas aplicaciones legítimas generan documentos, imágenes o tareas auxiliares. Baseline, allowlist y correlación con usuario, ruta y comando.

## 5. Comprobación posterior al parche

- [ ] Inventariar todas las instalaciones PHP, incluidas las incluidas en appliances, CMS y productos de terceros.
- [ ] Identificar versión efectiva, SAPI y proveedor del paquete.
- [ ] Aplicar el parche upstream o el backport oficial.
- [ ] Reiniciar todos los workers y servicios.
- [ ] Confirmar que los procesos activos usan la versión corregida.
- [ ] Verificar la configuración temporal y retirar excepciones innecesarias después del parche.
- [ ] Revisar logs, procesos, archivos modificados y conexiones salientes desde el periodo de exposición.
- [ ] Rotar credenciales y secretos si el proceso web pudo acceder a ellos.
- [ ] Registrar sistemas exceptuados y fecha límite para su actualización.

## Referencias

- [PHP - Security advisory, 21 October 2026](https://www.php.net/archive/2026.php#2026-10-21-5)
- [PHP - Informational advisory: Session upload progress](https://www.php.net/archive/2026.php#2026-10-23-3)
- [CERT-EU - Security Advisory 2026-018](https://cert.europa.eu/publications/security-advisories/2026-018/)
- [NCSC UK - Critical security issue affecting PHP](https://www.ncsc.gov.uk/news/critical-security-issue-affecting-php)
- [Elastic Security Labs - PHP2Shell detection coverage](https://www.elastic.co/security-labs/threat-command/php2shell-php-rce-detection)
- [Red Hat - RHSB-2026-013](https://access.redhat.com/security/vulnerabilities/RHSB-2026-013)
- [AWS Security Blog - PHP2Shell mitigations and EOL backports](https://aws.amazon.com/blogs/security/using-aws-waf-to-apply-mitigations-for-php2shell-rce-vulnerabilities-cve-2026-109618-and-cve-2026-109619/)
- [CISA - Critical Vulnerabilities in PHP Require Immediate Patching](https://content.govdelivery.com/accounts/USDHSCISA/bulletins/42ff5ac)
