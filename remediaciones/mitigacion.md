# Remediación, detección y recomendaciones

## Prioridad 1 | Actualizar PHP

Las versiones corregidas publicadas el 21 de octubre de 2026 son:

| Rama | Versión corregida |
|---|---:|
| PHP 8.5 | 8.5.12 |
| PHP 8.4 | 8.4.27 |
| PHP 8.3 | 8.3.36 |
| PHP 8.2 | 8.2.35 |

Verificar también los paquetes mantenidos por la distribución: algunos proveedores aplican backports sin cambiar necesariamente a la versión upstream indicada.

PHP 8.1 y anteriores están fuera del soporte ordinario del proyecto PHP. Migrar a una rama soportada o confirmar con el proveedor la existencia de un parche retroportado.

## Medidas temporales

Estas medidas reducen rutas de ataque concretas y no reemplazan el parche:

### Para CVE-2026-109618

- Evaluar la recomendación oficial sobre `memory_limit=-1` en configuración administrativa, considerando el riesgo de consumo de recursos.
- Limitar `post_max_size` según las necesidades de la aplicación.
- Mantener `upload_max_filesize` por debajo de `post_max_size`.
- Aplicar límites de tamaño del cuerpo, cantidad de campos, partes multipart y cabeceras en proxy/servidor web.
- Usar límites de contenedor o cgroup como frontera de disponibilidad y aislar el worker afectado.

**Precaución:** no sustituir estas medidas por `RLIMIT_AS` o `RLIMIT_DATA`; el aviso oficial advierte que pueden provocar fallos de asignación que alcancen la ruta vulnerable.

### Para CVE-2026-109619

- Configurar `session.upload_progress.enabled=0` antes del análisis de la petición.
- Reiniciar todos los workers después del cambio.
- En PHP 8.5, evaluar `fatal_error_backtraces=0` como reducción de una ruta específica.
- Corregir errores fatales y timeouts alcanzables desde peticiones remotas.

## Detección

Investigar especialmente:

- Procesos PHP que inician shells o intérpretes inesperados.
- Workers web que ejecutan herramientas de descarga o utilidades del sistema sin justificación.
- Solicitudes POST multipart anómalas o repetitivas.
- Errores fatales, OOM y reinicios inesperados de workers.
- Actividad sospechosa en registros del proxy, servidor web, PHP y sistema operativo.

Correlacionar hora, IP de origen, URI, worker, PID y procesos hijos. La ausencia de una firma conocida no demuestra que el sistema esté limpio.

## Lista de verificación

- [ ] Inventariar PHP y productos que lo integran.
- [ ] Identificar SAPI y versión efectiva en cada servicio.
- [ ] Aplicar actualización o parche del proveedor.
- [ ] Reiniciar workers y servicios afectados.
- [ ] Verificar versión y configuración posterior.
- [ ] Revisar registros y procesos anómalos.
- [ ] Documentar excepciones y sistemas aún expuestos.

## Fuentes

- [PHP Security Advisory](https://www.php.net/)
- [CISA](https://www.cisa.gov/news-events/alerts)
- [CERT-EU](https://cert.europa.eu/publications/security-advisories/2026-018/)
- [Elastic Security Labs](https://www.elastic.co/security-labs/threat-command/php2shell-php-rce-detection)