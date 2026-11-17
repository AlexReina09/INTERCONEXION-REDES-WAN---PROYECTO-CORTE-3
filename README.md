FlexARWAN

Tabla de contenido

1. Introducción	3
   
1.1 Objetivos	3

1.2 Alcance y limitaciones	3

3. Requerimientos de software	3
   
2.1 Requerimientos funcionales	3

2.2 Requerimientos no funcionales	4

5. Desarrollo del laboratorio	4
   
3.1 Arquitectura física y medios de conexión	4

3.2 Servicios de dispositivos	4

3.3 ISP, ancho de banda y redundancia	5

3.4 Protocolos por capa	5

3.5 Plan de direccionamiento y VLAN	5

3.6 Subnetting: ejemplo resuelto	6

3.7 Dispositivos recomendados por piso	6

3.8 Configuración por fabricante	6

3.9 Ciberdefensa y VPN	7

3.10 Panel IA local	7

3.11 Pruebas de failover	7

3.12 Entrenamiento supervisado	7

3.13 Control de versiones y repositorio	7

7. Conclusiones	8
   
9. Bibliografía	8

----------------------------------------------------------------------------
----------------------------------------------------------------------------

1. Introducción:
   
Un edificio de 20 pisos con servicios de datos, voz, video, WiFi y videovigilancia (CCTV) necesita una red ordenada, segura y con respaldo. Configurar a mano switches, routers, puntos de acceso y firewalls de varios fabricantes genera errores y diferencias entre pisos. FlexARWAN es una herramienta web, autónoma y de ejecución local, que calcula subredes, planifica el direccionamiento por piso, importa el inventario, genera configuraciones iniciales y simula fallos de enlace.

1.1 Objetivos

•	Calcular subredes IPv4 correctamente (red, broadcast y hosts útiles) y planificar IPv4 e IPv6 por piso y por servicio.
•	Generar configuraciones iniciales para Cisco, Huawei, Fortinet, MikroTik y HP a partir de un único plan.
•	Detectar configuraciones inseguras con reglas locales, sin enviar datos fuera del equipo.
•	Simular la caída de enlaces y proveedores (ISP) para evaluar la redundancia por piso.

1.2 Alcance y limitaciones

La versión V0.0.0 entrega una herramienta funcional como página HTML. Los scripts son una base inicial por familia de fabricante: deben validarse en laboratorio contra el modelo y el firmware reales antes de producción. Ningún sistema puede garantizar una confiabilidad del 100 %; el diseño reduce el riesgo mediante redundancia, validación de entradas y pruebas, pero no lo elimina. El agente con IA, el acceso SSH remoto automatizado, la VPN configurable y la vista por capas de protocolos están planificados para versiones posteriores.

3. Requerimientos de software

2.1 Requerimientos funcionales


<img width="763" height="412" alt="image" src="https://github.com/user-attachments/assets/c882c161-a18f-4ccb-817f-ca5ad9e0b39f" />


2.2 Requerimientos no funcionales

•	Seguridad: sin envío de datos a terceros; contraseñas generadas con un generador aleatorio criptográfico del navegador.

•	Usabilidad: interfaz simple, consistente, de claridad visual y con retroalimentación inmediata mediante mensajes.

•	Portabilidad y responsividad: un solo archivo HTML, con tema claro y oscuro y adaptación a pantallas pequeñas.

•	Mantenibilidad: código comentado en español y control de versiones (V0.0.0).

•	Escalabilidad: parámetros editables (VLAN, prefijos) y soporte para el inventario de los 20 pisos.

3. Desarrollo del laboratorio

3.1 Arquitectura física y medios de conexión

El diseño usa una topología jerárquica de tres capas simplificada: acceso en cada piso, distribución en el piso de equipos y core redundante (Core A y Core B). Cada piso se conecta con dos enlaces de fibra, uno a cada core.


<img width="639" height="244" alt="image" src="https://github.com/user-attachments/assets/2d9a3366-bf7b-43a0-8aec-e31d6d8c67a2" />


3.2 Servicios de dispositivos

Los equipos físicos (routers, switches, puntos de acceso, firewalls, teléfonos y cámaras IP) y lógicos (DHCP, DNS, NAT, VPN, ACL, QoS, monitoreo y registros) conectan, gestionan y protegen la comunicación entre computadoras y sistemas. El router enruta entre redes y hacia el ISP; el switch conmuta tramas y segmenta con VLAN; el punto de acceso extiende la red de forma inalámbrica; el firewall filtra y aplica políticas; el teléfono IP y la cámara IP son terminales que exigen priorización (voz) o segmentación estricta (CCTV).

3.3 ISP, ancho de banda y redundancia

Se recomienda contratar dos ISP con acometidas físicas distintas y, si es posible, con ancho de banda simétrico garantizado por contrato (SLA). El dimensionamiento inicial debe medirse con tráfico real; como referencia de diseño se calcula el consumo por servicio (datos, voz, video, CCTV) y se agrega un margen de crecimiento. Los firewalls operan en alta disponibilidad y el simulador muestra el efecto de perder un ISP: de «redundante» a «sin redundancia» y, con ambos caídos, «sin Internet».

3.4 Protocolos por capa


<img width="639" height="170" alt="image" src="https://github.com/user-attachments/assets/dd55e2d2-8044-4b25-8fdc-c4c015ea2349" />


3.5 Plan de direccionamiento y VLAN

Cada servicio usa el esquema 10.<piso>.<tercer octeto>.0/<prefijo>. En IPv6 se usa 2001:db8:<piso>:<VLAN>::/64; ese prefijo está reservado para documentación (RFC 3849) y debe reemplazarse por el entregado por el ISP.

<img width="640" height="181" alt="image" src="https://github.com/user-attachments/assets/2eb253ea-1b9a-4714-a8ec-662fa2572a8d" />


3.6 Subnetting: ejemplo resuelto


<img width="649" height="209" alt="image" src="https://github.com/user-attachments/assets/00dd1efa-38fd-4925-b86c-4de90625bc43" />



Verificación: el incremento es 256 − 192 = 64, así que las subredes comienzan en 0, 64, 128 y 192. Esto coincide con la salida de la calculadora de FlexARWAN.

3.7 Dispositivos recomendados por piso

•	2 switches de acceso PoE de 48 puertos (principal y respaldo) con 2 uplinks de fibra de 10 G.

•	Puntos de acceso Wi-Fi: 2 por cada 500 m² de área útil.

•	Teléfonos IP: uno por puesto de trabajo con voz; cámaras IP: aproximadamente 1 por cada 80 m².

•	Piso de equipos: 2 routers WAN, 2 firewalls en alta disponibilidad y 2 switches core.

Estas cifras son una guía inicial y deben ajustarse al levantamiento real de cada piso.


<img width="645" height="260" alt="image" src="https://github.com/user-attachments/assets/32f58498-b1cf-4b7e-a8d6-0960abcfe044" />


3.8 Configuración por fabricante

<img width="643" height="264" alt="image" src="https://github.com/user-attachments/assets/2cc2485d-0922-44e5-a499-b0c0ac99dc78" />


La herramienta distingue el fabricante seleccionado y genera la sintaxis correspondiente. Los puertos de uplink (por ejemplo TenGigabitEthernet1/1/1-2 en Cisco o puertos 49–52 en HP) son valores de ejemplo y deben ajustarse al modelo exacto con ayuda del inventario CSV.

3.9 Ciberdefensa y VPN

•	Acceso administrativo solo por SSH versión 2; Telnet desactivado en producción por enviar datos en texto claro.

•	VLAN de gestión aislada (99) y listas de control de acceso entre VLAN con el principio de mínimo privilegio.

•	802.1X en puertos de acceso, DHCP snooping y Dynamic ARP Inspection contra suplantación.

•	VPN IPsec con IKEv2, cifrado AES-256-GCM e integridad SHA-384, siguiendo las guías del NIST.

•	Contra ataques asistidos por IA (por ejemplo, phishing o fuerza bruta automatizada): MFA, 
bloqueo temporal por intentos, segmentación de CCTV y VoIP, registros centralizados y detección de anomalías.

3.10 Panel IA local

El panel aplica reglas locales (expresiones regulares) sobre la configuración pegada por el usuario. Detecta Telnet activo, comunidades SNMP por defecto, contraseñas débiles, servicios HTTP/FTP sin cifrar, reglas «permitir todo» y ausencia de SSH o de protecciones de capa 2. No usa un modelo remoto, por lo que ningún dato sale del equipo. No sustituye una auditoría profesional.

3.11 Pruebas de failover


<img width="644" height="159" alt="image" src="https://github.com/user-attachments/assets/b9cc74c1-d2fd-4850-87e9-62aebd3623cf" />


3.12 Entrenamiento supervisado

Para el entrenamiento supervisado de las reglas de auditoría, se propone un conjunto de configuraciones etiquetadas por un especialista (seguras e inseguras). Cada regla se evalúa contra ese conjunto midiendo falsos positivos y falsos negativos, y el especialista revisa y corrige las reglas antes de publicar una nueva versión. Cuando se incorpore un modelo de IA, este flujo de revisión humana se mantiene como requisito.


3.13 Control de versiones y repositorio

La versión actual es V0.0.0. Se propone un repositorio público en GitHub con README, una rama de trabajo (develop) además de main y al menos cinco commits con mensajes claros, por ejemplo: «Agrega calculadora de subredes IPv4», «Agrega editor de VLAN y plan por piso», «Agrega generador de configuración multifabricante», «Agrega panel de auditoría local» y «Agrega simulador de failover».

4. Conclusiones

•	La automatización del plan IP y de las configuraciones reduce errores manuales y mantiene la coherencia entre los 20 pisos.

•	Un único plan de VLAN y direccionamiento permite generar sintaxis para varios fabricantes, aunque cada modelo requiere validación en laboratorio.

•	La redundancia debe probarse: el simulador muestra que un solo core o ISP caído deja el servicio, pero sin respaldo.

•	La auditoría local mejora la privacidad al no enviar configuraciones a terceros, pero sus reglas deben ampliarse y supervisarse.

•	Trabajo futuro: agente con IA y SSH, VPN configurable, vista por capas de protocolos y pruebas con equipos reales.

5. Bibliografía

   
Cisco Systems. (s. f.). Documentación oficial de Cisco IOS XE y Catalyst. Cisco Documentation. https://www.cisco.com/c/en/us/support/index.html
Deering, S., & Hinden, R. (2017). Internet Protocol, Version 6 (IPv6) Specification (RFC 8200). IETF.
Fortinet. (s. f.). FortiOS Administration Guide. Fortinet Document Library. https://docs.fortinet.com
Fuller, V., & Li, T. (2006). Classless Inter-domain Routing (CIDR) (RFC 4632). IETF.
Huawei Technologies. (s. f.). Documentación de productos de redes empresariales (VRP). Huawei Support. https://support.huawei.com
IEEE. (2018). IEEE Std 802.1Q-2018: Bridges and Bridged Networks. IEEE.
IEEE. (2020). IEEE Std 802.1X-2020: Port-Based Network Access Control. IEEE.
Kaufman, C., Hoffman, P., Nir, Y., Eronen, P., & Kivinen, T. (2014). Internet Key Exchange Protocol Version 2 (IKEv2) (RFC 7296). IETF.
Kurose, J. F., & Ross, K. W. (2021). Computer Networking: A Top-Down Approach (8.ª ed.). Pearson.
MikroTik. (s. f.). RouterOS Manual. MikroTik Documentation. https://help.mikrotik.com/docs
National Institute of Standards and Technology. (2020). Security and Privacy Controls for Information Systems and Organizations (SP 800-53, Rev. 5). NIST.
National Institute of Standards and Technology. (2020). Guide to IPsec VPNs (SP 800-77, Rev. 1). NIST.
Rekhter, Y., Moskowitz, B., Karrenberg, D., de Groot, G. J., & Lear, E. (1996). Address Allocation for Private Internets (RFC 1918). IETF.
Huston, G., Lord, A., & Smith, P. (2004). IPv6 Address Prefix Reserved for Documentation (RFC 3849). IETF.
Ylonen, T., & Lonvick, C. (2006). The Secure Shell (SSH) Protocol Architecture (RF.


Capturas De pantalla Módulos Herramienta:


<img width="975" height="759" alt="image" src="https://github.com/user-attachments/assets/1e1e6ced-0c1e-45ce-b9cb-872e064caa3d" />


<img width="975" height="429" alt="image" src="https://github.com/user-attachments/assets/04fdd715-1b8b-43f5-a9c3-0343fdeb720c" />


<img width="1086" height="547" alt="image" src="https://github.com/user-attachments/assets/eadfa608-a978-4312-852c-b75124cbe901" />


<img width="1106" height="577" alt="image" src="https://github.com/user-attachments/assets/791748a1-cc7b-4252-817f-e2c76e791b44" />


<img width="1092" height="390" alt="image" src="https://github.com/user-attachments/assets/78c62790-d072-445c-9d21-718c0bce0376" />


<img width="1098" height="575" alt="image" src="https://github.com/user-attachments/assets/21c9a456-8907-4c77-a5aa-89e52ee1a3bc" />


<img width="1105" height="557" alt="image" src="https://github.com/user-attachments/assets/1f708c9d-62d2-4621-8917-22b6bb2d9be1" />


<img width="1170" height="502" alt="image" src="https://github.com/user-attachments/assets/b383c45d-8877-4f4a-af2a-cf6878f4fdac" />


<img width="1079" height="464" alt="image" src="https://github.com/user-attachments/assets/60be0cdd-06d6-419b-b4fd-ec23c2b36df4" />
