# FlexARWAN

Herramienta web de **un solo archivo** (`index.html`) para planificar y configurar la red de un edificio de 20 pisos. Funciona en el navegador, sin instalación, sin servidor y sin enviar datos a terceros.

> **Versión actual:** 0.0.0 (primera entrega). Revisa siempre las configuraciones generadas en laboratorio antes de aplicarlas en producción.

Proyecto académico de Interconexión de Redes WAN · Fundación Universitaria Ucompensar.

## Funciones

| Pestaña | Qué hace |
|---|---|
| **Subnetting** | Calcula red, máscara, broadcast, primer/último host y hosts útiles (IPv4), y lista subredes /64 (IPv6). Permite dividir una red en 1, 4, 8, 16, 32 o 64 subredes. |
| **Plan IP y VLAN** | Plan editable por piso y servicio (datos, voz, video, WiFi, CCTV): VLAN, CIDR IPv4, pool DHCP e IPv6 /64. Valida los valores mientras se escriben. |
| **Inventario CSV** | Importa la lista de equipos (hostname, fabricante, modelo, piso, puertos, rol). Si falta el número de puertos, lo deduce del modelo. |
| **Configuración** | Genera scripts iniciales para **Cisco, Huawei, Fortinet, MikroTik y HP/Aruba**, por piso o para los 20 a la vez. Copiar o descargar como `.txt`. |
| **Failover** | Simula la caída de ISP 1, ISP 2, Uplink core A y Uplink core B, y muestra el estado de los 20 pisos. |
| **Panel IA local** | Revisa una configuración con reglas locales (expresiones regulares). El análisis ocurre en tu navegador. |
| **Acceso** | Genera usuario y contraseña aleatorios con `crypto.getRandomValues`. No se guardan en el proyecto. |

En la barra superior: **Guardar proyecto**, **Descargar JSON** y **Descargar README.md**.

## Uso rápido

1. Descarga `index.html`.
2. Ábrelo en un navegador moderno (doble clic).
3. Calcula subredes, ajusta el plan, importa tu inventario y genera las configuraciones.

No requiere `npm`, compilación ni dependencias.

## Despliegue en GitHub Pages

1. Sube el repositorio a GitHub.
2. En **Settings → Pages**, elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`.
3. Tu app quedará en `https://<tu-usuario>.github.io/<nombre-del-repositorio>/`.

## Formato del CSV de inventario

```csv
hostname,fabricante,modelo,piso,puertos,rol
SW-P1,Cisco,Catalyst 9300-48,1,,switch
SW-P2,Huawei,S5735-L24,2,,switch
FW-CORE,Fortinet,FortiGate 100F,0,,firewall
RT-EDGE,MikroTik,CCR2004,0,12,router
AP-P1,HP,Aruba AP-515,1,2,access point
```

- La primera línea (encabezado) es opcional; se omite si contiene `hostname`.
- Si `puertos` está vacío, se busca 48, 24, 16 u 8 en el nombre del modelo.
- El separador es la coma. No se admiten campos entre comillas que contengan comas.
- El botón **Cargar ejemplo** carga el contenido de arriba.

## Plan por defecto

Para el piso `f` (1 a 20) la herramienta crea cinco servicios:

| Servicio | VLAN | CIDR IPv4 | IPv6 /64 |
|---|---|---|---|
| datos | 100 + f | 10.f.0.0/24 | 2001:db8:f:0::/64 |
| voz | 200 + f | 10.f.10.0/24 | 2001:db8:f:1::/64 |
| video | 300 + f | 10.f.20.0/24 | 2001:db8:f:2::/64 |
| wifi | 400 + f | 10.f.30.0/24 | 2001:db8:f:3::/64 |
| cctv | 500 + f | 10.f.40.0/24 | 2001:db8:f:4::/64 |

Ejemplo, piso 5: datos = VLAN 105, 10.5.0.0/24. El número de piso va en hexadecimal dentro de la dirección IPv6. El pool DHCP se calcula como `red + 10` hasta `broadcast − 5` cuando la subred tiene 20 hosts o más.

> El prefijo `2001:db8::/32` está reservado para documentación (RFC 3849). Reemplázalo por el que te asigne tu ISP.

## Endurecimiento incluido en los scripts

| Fabricante | Medidas |
|---|---|
| Cisco | SSH versión 2, HTTP/HTTPS del servidor desactivados, `line vty` solo SSH con `login local`, tiempo de inactividad de 10 min. |
| Huawei | `stelnet` activo, telnet desactivado, VTY con AAA y solo SSH. |
| HP / Aruba | `no telnet-server`, SSH activo, gestión web desactivada. |
| MikroTik | Desactiva telnet, ftp, www, api y api-ssl; SSH en el puerto 22. |
| Fortinet | Sin bloque adicional; la interfaz permite `ping https ssh`. |

## Reglas del Panel IA local

Detecta: Telnet, comunidades SNMP `public`/`private`, contraseñas por defecto, reglas `permit any any` o `accept all`, administración HTTP, FTP y uso de la VLAN 1. Además avisa si no hay SSH ni trunks con VLAN permitidas explícitas.

No es un modelo de IA remoto ni sustituye una auditoría profesional.

## Limitaciones conocidas (V0.0.0)

- **No se conecta a los equipos.** SSH y Telnet no pueden abrirse desde una página web. Copia el script y pégalo en la consola, o usa un agente local como Ansible o Netmiko.
- Los **puertos de uplink** de los scripts son valores de ejemplo (por ejemplo, `Gi1/0/47-48` en Cisco). Ajústalos al modelo real.
- El **Failover** es una simulación simplificada: aplica el mismo estado a todos los pisos.
- El proyecto se guarda en el `localStorage` del navegador (clave `flexarwan`); otro navegador o equipo no verá los datos. Usa **Descargar JSON** como respaldo.
- Las fuentes Manrope y JetBrains Mono se cargan desde Google Fonts. Sin conexión, el navegador usa fuentes del sistema.

## Estructura del repositorio

```text
.
├── index.html     # La aplicación completa (HTML + CSS + JS)
├── README.md
├── CHANGELOG.md
└── .gitignore
```

## Hoja de ruta

Funciones previstas (ver [CHANGELOG](CHANGELOG.md)): agente con IA y conexión SSH, configuración de VPN IPsec/IKEv2 y vista de protocolos por capa.

## Versionado

Se sigue [Versionado Semántico](https://semver.org/lang/es/). El historial está en [CHANGELOG.md](CHANGELOG.md).

## Licencia

Por definir.
