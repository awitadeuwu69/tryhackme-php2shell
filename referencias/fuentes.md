# Referencias y fuentes

## Fuentes primarias

1. [TryHackMe - PHP2Shell](https://tryhackme.com/room/cvephp2shell) - objetivos y entorno del laboratorio.
2. [PHP Security Advisory, 21 October 2026](https://www.php.net/archive/2026.php#2026-10-21-5) - causa raíz, versiones corregidas y mitigaciones temporales para ambas CVE.
3. [GHSA-w8hw-w9fx-j79w - CVE-2026-109618](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w) - aviso de seguridad y detalles técnicos.
4. [GHSA-6wmf-4rv5-p3rp - CVE-2026-109619](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp) - aviso de seguridad y detalles técnicos.

## Investigación técnica

5. [Calif Research - PHP2Shell: A tale of two PHP remote vulnerabilities](https://calif.io/php2shell/) - análisis técnico e investigación.
6. [Elastic Security Labs - PHP2Shell detection coverage](https://www.elastic.co/security-labs/threat-command/php2shell-php-rce-detection) - detección basada en comportamiento del proceso web y post-explotación.
7. [Red Hat - RHSB-2026-013](https://access.redhat.com/security/vulnerabilities/RHSB-2026-013) - análisis de impacto y diferencias de exposición según el despliegue.
8. [AWS Security Blog - PHP2Shell mitigations](https://aws.amazon.com/blogs/security/using-aws-waf-to-apply-mitigations-for-php2shell-rce-vulnerabilities-cve-2026-109618-and-cve-2026-109619/) - mitigaciones de infraestructura y WAF.

## Guías de respuesta

9. [CERT-EU - Security Advisory 2026-018](https://cert.europa.eu/publications/security-advisories/2026-018/) - recomendaciones de seguridad.
10. [NCSC UK - Critical security issue affecting PHP](https://www.ncsc.gov.uk/news/critical-security-issue-affecting-php) - inventario, priorización, actualización y análisis de compromiso.
11. [CISA - Critical Vulnerabilities in PHP Require Immediate Patching](https://content.govdelivery.com/accounts/USDHSCISA/bulletins/42ff5ac) - recomendación de actualización urgente y vigilancia de procesos web sospechosos.

## Criterios de uso

- Priorizar PHP y los avisos GHSA para causa raíz, versiones y mitigaciones.
- Usar análisis de terceros para ampliar detección y contexto, sin sustituir la fuente primaria.
- Distinguir hechos confirmados, inferencias técnicas y observaciones específicas del laboratorio.
- Los offsets y direcciones de memoria del exploit son propios del build probado y no son universales.

## Fecha de consulta

2026-11-08.
