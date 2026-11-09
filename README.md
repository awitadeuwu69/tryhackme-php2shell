# TryHackMe | PHP2Shell

Write-up técnico de la sala **[PHP2Shell](https://tryhackme.com/room/cvephp2shell)** de TryHackMe.

**Estado:** completado

## Objetivos

- Explicar el funcionamiento de `session.upload_progress` y su relación con el ciclo de vida de las peticiones PHP.
- Comprender los errores de gestión de memoria asociados a PHP2Shell.
- Diferenciar CVE-2026-109618 y CVE-2026-109619.
- Analizar la cadena de explotación en el entorno controlado de TryHackMe.
- Identificar señales de compromiso y documentar medidas defensivas.

## Vulnerabilidades

| Identificador | Causa raíz | Severidad |
|---|---|---|
| [CVE-2026-109618](vulnerabilidades/CVE-2026-109618.md) | Corrupción durante el manejo de errores de memoria insuficiente (OOM). | Crítica, CVSS 4.0: 9.2 |
| [CVE-2026-109619](vulnerabilidades/CVE-2026-109619.md) | Estado de petición obsoleto que conserva referencias a memoria liberada. | Crítica, CVSS 4.0: 9.2 |

Ambas vulnerabilidades son descritas por PHP como posibles vías de ejecución remota de código. Las fugas de memoria y las direcciones observadas durante el exploit son primitivas de la cadena de explotación, no una clasificación alternativa de las CVE.

## Estructura

- `reconocimiento/`: enumeración del objetivo.
- `vulnerabilidades/`: análisis técnico de cada CVE y de las primitivas.
- `evidencias/imagenes/`: capturas del laboratorio.
- `explotacion/`: procedimiento y resultados reproducibles.
- `remediaciones/`: mitigación, detección y respuesta.
- `referencias/`: fuentes primarias y análisis complementarios.

## Resultados del laboratorio

- Máquina TryHackMe: Ubuntu, usuario de sesión `ubuntu`.
- Contenedor objetivo: `php2shell-target`, imagen `php:8.4.26-apache`.
- Versión PHP comprobada dentro del contenedor: `PHP 8.4.26 (cli)`.
- Servicio HTTP local: `http://127.0.0.1:8088/index.php`.
- La petición de comprobación respondió `OK`.
- El exploit completó las etapas de fuga de heap, fuga de dirección de `libphp` y ejecución.
- Prueba de ejecución: `uid=33(www-data) gid=33(www-data) groups=33(www-data)`.
- TryHackMe mostró **Room completed**, con 8 tareas completadas y 80 puntos.

**Alcance:** se confirmó ejecución de comandos con el usuario del servicio web `www-data` dentro del contenedor. No se obtuvo evidencia de root ni de escape del contenedor. La flag fue validada en la plataforma y se omite del repositorio público. Las capturas se almacenan únicamente en `evidencias/imagenes/`; revisa que ninguna imagen exponga la flag, cookies, credenciales o tokens antes de mantenerla pública.

## Análisis técnico

- [CVE-2026-109618: corrupción OOM](vulnerabilidades/CVE-2026-109618.md)
- [CVE-2026-109619: estado global obsoleto](vulnerabilidades/CVE-2026-109619.md)
- [Detección y remediación](remediaciones/mitigacion.md)

## Alcance y ética

Todas las pruebas se limitaron a la máquina proporcionada por TryHackMe. No ejecutar estas técnicas contra sistemas ajenos ni expuestos a Internet sin autorización explícita.

## Referencias principales

- [Sala PHP2Shell - TryHackMe](https://tryhackme.com/room/cvephp2shell)
- [Aviso oficial de PHP](https://www.php.net/archive/2026.php#2026-10-21-5)
- [CERT-EU Security Advisory 2026-018](https://cert.europa.eu/publications/security-advisories/2026-018/)
- [Elastic Security Labs - PHP2Shell detection](https://www.elastic.co/security-labs/threat-command/php2shell-php-rce-detection)
- [NCSC - Critical security issue affecting PHP](https://www.ncsc.gov.uk/news/critical-security-issue-affecting-php)

---
Write-up educativo. Los resultados específicos corresponden a las evidencias obtenidas en la instancia del laboratorio.
