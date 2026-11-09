# TryHackMe | PHP2Shell

Write-up técnico de la sala **[PHP2Shell](https://tryhackme.com/room/cvephp2shell)** de TryHackMe.

> **Estado:** sala completada. Resultados y evidencias incorporados a partir de la ejecución real en el laboratorio.

## Objetivos

- Explicar el funcionamiento de `session.upload_progress` y su relación con el ciclo de vida de las peticiones PHP.
- Comprender los errores de gestión de memoria asociados a PHP2Shell.
- Diferenciar CVE-2026-109618 y CVE-2026-109619.
- Analizar la cadena de explotación en el entorno controlado de TryHackMe.
- Identificar señales de compromiso y documentar medidas defensivas.

## Vulnerabilidades

| Identificador | Descripción oficial resumida | Severidad |
|---|---|---|
| [CVE-2026-109618](https://www.php.net/) | Ejecución remota de código por corrupción durante el manejo de errores de memoria insuficiente (OOM). | Crítica, CVSS 4.0: 9.2 |
| [CVE-2026-109619](https://www.php.net/) | Ejecución remota de código mediante estado global obsoleto que conserva referencias a memoria liberada. | Crítica, CVSS 4.0: 9.2 |

**Nota de precisión:** el aviso oficial de PHP describe ambas vulnerabilidades como vías potenciales de ejecución remota de código. En el análisis práctico se explicarán por separado las primitivas de filtración de memoria, corrupción y ejecución que aparecen en la cadena didáctica de la sala. No se debe interpretar que una CVE sea exclusivamente una vulnerabilidad de divulgación de información.

## Estructura

- `reconocimiento/`: contexto y enumeración del objetivo.
- `vulnerabilidades/`: análisis técnico individual de cada CVE.
- `evidencias/`: capturas y resultados obtenidos durante la práctica.
- `explotacion/`: procedimiento reproducible dentro del laboratorio.
- `remediaciones/`: mitigaciones, detección y recomendaciones.
- `referencias/`: fuentes primarias y documentación complementaria.

## Metodología

Cada fase sigue el mismo esquema:

1. **Objetivo:** qué se intenta comprobar.
2. **Procedimiento:** comandos y pasos realmente ejecutados.
3. **Resultado:** salida observada, sin datos inventados.
4. **Evidencia:** captura asociada y contexto.
5. **Análisis:** explicación técnica del resultado.
6. **Impacto y recomendaciones:** consecuencias y controles defensivos.

## Resultados del laboratorio

- Máquina TryHackMe: Ubuntu, usuario de sesión `ubuntu`.
- Contenedor objetivo: `php2shell-target`, imagen `php:8.4.26-apache`.
- Versión PHP comprobada dentro del contenedor: `PHP 8.4.26 (cli)`.
- Servicio HTTP local: `http://127.0.0.1:8088/index.php`.
- La petición de comprobación respondió `OK`.
- El exploit completó las etapas de fuga de heap, fuga de dirección de `libphp` y ejecución.
- Prueba de ejecución: `uid=33(www-data) gid=33(www-data) groups=33(www-data)`.
- La sala mostró el estado **Room completed** y los 8 objetivos completados.

**Alcance del resultado:** se confirmó ejecución de comandos con el usuario del servicio web `www-data`. Esto no equivale a obtener root ni control del host Ubuntu.

La flag se verificó en la sesión, pero se omite deliberadamente de este repositorio público.

## Progreso

- [x] Crear repositorio y estructura inicial.
- [x] Documentar contexto y fuentes oficiales.
- [x] Reconocimiento del objetivo del laboratorio.
- [x] Identificar contenedor, servicio y versión de PHP.
- [x] Reproducir la cadena de explotación.
- [x] Verificar ejecución de comandos como `www-data`.
- [x] Completar la sala de TryHackMe.
- [ ] Ampliar análisis técnico de cada primitiva y su relación con las CVE.
- [ ] Completar detección y remediación con referencias contrastadas.

## Alcance y ética

Todas las pruebas de explotación se limitaron a la máquina y los servicios proporcionados por TryHackMe. No ejecutar estas técnicas contra sistemas ajenos ni expuestos a Internet sin autorización explícita.

No se publican flags, credenciales, tokens ni secretos del laboratorio.

## Referencias principales

- [Sala PHP2Shell - TryHackMe](https://tryhackme.com/room/cvephp2shell)
- [Avisos y releases de seguridad - PHP](https://www.php.net/)
- [Análisis técnico - Calif Research](https://calif.io/php2shell/)
- [CERT-EU Security Advisory 2026-018](https://cert.europa.eu/publications/security-advisories/2026-018/)
- [Elastic Security Labs - PHP2Shell](https://www.elastic.co/security-labs/threat-command/php2shell-php-rce-detection)
- [CISA - Critical Vulnerabilities in PHP](https://www.cisa.gov/news-events/alerts)

---
Write-up educativo. Los resultados específicos aquí consignados corresponden a las evidencias obtenidas en la instancia del laboratorio.
