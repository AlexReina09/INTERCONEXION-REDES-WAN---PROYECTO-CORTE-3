# Historial de cambios

Todos los cambios relevantes de este proyecto se documentan aquí.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y el proyecto usa [Versionado Semántico](https://semver.org/lang/es/).

## [Sin publicar]

### Planificado
- Agente con IA y conexión SSH a los equipos.
- Configuración de VPN IPsec/IKEv2 desde la herramienta.
- Vista de protocolos por capa asociada a cada equipo.

## [0.0.0] - 2026-11-16

Primera entrega: aplicación de un solo archivo (`index.html`), sin dependencias de compilación. La interfaz muestra la versión como "V0.0".

### Añadido
- **Subnetting IPv4**: red, máscara, broadcast, primer/último host y hosts útiles; división en 1, 4, 8, 16, 32 o 64 subredes (tabla de hasta 64 filas).
- **Subnetting IPv6** con aritmética `BigInt`: red, primera/última dirección, total de direcciones y listado de subredes /64.
- **Plan IP y VLAN** para 20 pisos y 5 servicios (datos, voz, video, WiFi, CCTV), con VLAN, CIDR IPv4 y prefijo IPv6 editables, pool DHCP calculado y validación en línea.
- **Inventario CSV**: importación con columnas hostname, fabricante, modelo, piso, puertos y rol; reconocimiento de fabricante; deducción de puertos por modelo; botón "Cargar ejemplo".
- **Generador de configuraciones** para Cisco, Huawei, Fortinet, MikroTik y HP/Aruba, por piso o para todos los pisos, con copiar y descargar `.txt`, y bloque de endurecimiento por fabricante.
- **Simulador de failover** con ISP 1, ISP 2, Uplink core A y Uplink core B, y estado de los 20 pisos.
- **Panel IA local** basado en reglas: Telnet, SNMP por defecto, contraseñas débiles, reglas permisivas, HTTP, FTP, VLAN 1, ausencia de SSH y de trunks explícitos.
- **Generador de credenciales** con `crypto.getRandomValues` (usuario `adm-xxxxxx` y contraseña de 20 caracteres); no se guardan en el proyecto.
- **Guardado y exportación**: "Guardar proyecto" en `localStorage`, "Descargar JSON" y "Descargar README.md".
- Tema claro y oscuro según la preferencia del sistema, diseño adaptable y navegación accesible por teclado.

### Limitaciones conocidas
- No se conecta a los equipos (SSH/Telnet no están disponibles desde una página web).
- Los puertos de uplink de los scripts son valores de ejemplo.
- Las fuentes se cargan desde Google Fonts; sin conexión se usan las del sistema.

[Sin publicar]: https://github.com/USUARIO/REPOSITORIO/compare/v0.0.0...HEAD
[0.0.0]: https://github.com/USUARIO/REPOSITORIO/releases/tag/v0.0.0
