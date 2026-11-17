# FlexARWANV0.0 · edificio de 20 pisos

## Subnetting IPv4

Red / prefijo Dividir en

## Subnetting IPv6

Red / prefijo Mostrar /64

## Plan IP y VLAN por piso

Edita VLAN, CIDR IPv4 y prefijo IPv6 de cada servicio. Los cambios se aplican a las configuraciones generadas.

Piso

| Servicio | VLAN | CIDR IPv4 | Pool DHCP | IPv6 /64 |
| --- | --- | --- | --- | --- |

## Importar inventario CSV

Columnas: hostname, fabricante, modelo, piso, puertos, rol. Si falta el número de puertos, se deduce del modelo.

| Equipo | Fabricante | Modelo | Piso | Puertos | Rol |
| --- | --- | --- | --- | --- | --- |

Sin equipos cargados.

## Generar configuración

Fabricante Piso

## Prueba de failover

Desmarca un enlace para simular su caída y observa el estado de cada piso.

[x] ISP 1 [x] ISP 2 [x] Uplink core A [x] Uplink core B

## Panel IA local

Pega una configuración o usa la generada. El análisis ocurre en tu navegador.

## Usuario y contraseña seguros

Usuario

—

Contraseña

—

No se guardan en el proyecto. Cópialas a tu gestor de contraseñas.

## Alcance de esta versión

SSH y Telnet no se pueden abrir desde una página web. Para aplicar los scripts a los equipos, copia la configuración y pégala en la consola, o usa un agente local (por ejemplo Ansible o Netmiko).