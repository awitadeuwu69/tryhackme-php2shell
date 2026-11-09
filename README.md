# TryHackMe | PHP2Shell

Write-up técnico de la sala **[PHP2Shell](https://tryhackme.com/room/cvephp2shell)** de TryHackMe.

> **Estado:** documentación inicial. Las evidencias de ejecución y los resultados del laboratorio se añadirán únicamente después de reproducirlos.

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

Cada fase seguirá el mismo esquema:

1. **Objetivo:** qué se intenta comprobar.
2. **Procedimiento:** comandos y pasos realmente ejecutados.
3. **Resultado:** salida observada, sin datos inventados.
4. **Evidencia:** captura asociada y contexto.
5. **Análisis:** explicación técnica del resultado.
6. **Impacto y recomendaciones:** consecuencias y controles defensivos.

## Alcance y ética

Todas las pruebas de explotación se limitan a la máquina y los servicios proporcionados por TryHackMe. No ejecutar estas técnicas contra sistemas ajenos ni expuestos a Internet sin autorización explícita.

Las carpetas de evidencias están preparadas para incorporar capturas originales. No se incluyen flags, direcciones de objetivo ni resultados de explotación ficticios.

## Progreso

- [x] Crear repositorio y estructura inicial.
- [x] Documentar contexto y fuentes oficiales.
- [ ] Reconocimiento del objetivo del laboratorio.
- [ ] Análisis del mecanismo de PHP.
- [ ] Reproducción de la cadena de explotación.
- [ ] Evidencia de acceso y lectura de flag.
- [ ] Detección y remediación.

## Referencias principales

- [Sala PHP2Shell - TryHackMe](https://tryhackme.com/room/cvephp2shell)
- [Avisos y releases de seguridad - PHP](https://www.php.net/)
- [Análisis técnico - Calif Research](https://calif.io/php2shell/)
- [CERT-EU Security Advisory 2026-018](https://cert.europa.eu/publications/security-advisories/2026-018/)
- [Elastic Security Labs - PHP2Shell](https://www.elastic.co/security-labs/threat-command/php2shell-php-rce-detection)
- [CISA - Critical Vulnerabilities in PHP](https://www.cisa.gov/news-events/alerts)

---
**Nota:** write-up educativo en desarrollo. Las conclusiones específicas del laboratorio se actualizarán a medida que se obtengan evidencias.