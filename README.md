# CFI-2-Redes
# Documentación CFI2 - Plataforma de Transferencia de Archivos y Multimedia en Entorno Heterogéneo

**Autores:**  
María Díaz, Juan Pablo Lobato, Mauricio Murillo, Cintia Santillán, Jiachen Ye  
**Universidad:** Alfonso X El Sabio  

---

## Índice
1. [Definición del Modelo de Comunicación](#1-definición-del-modelo-de-comunicación)  
2. [Capa Física – Capacidad y Modulación](#2-capa-física--capacidad-y-modulación)  
3. [Capa de Red – Subneteo y Enrutamiento](#3-capa-de-red--subneteo-y-enrutamiento)  
4. [Capa de Transporte – Selección y Cálculo de Ventana](#4-capa-de-transporte--selección-y-cálculo-de-ventana)  
5. [Capa de Aplicación – Servicios y Multiplexación](#5-capa-de-aplicación--servicios-y-multiplexación)  
6. [Multimedia](#6-multimedia)  
7. [Seguridad – Estrategias y Configuraciones](#7-seguridad--estrategias-y-configuraciones)  
8. [Implementación en Cisco Packet Tracer](#8-implementación-en-cisco-packet-tracer)  

---

### 1. Definición del Modelo de Comunicación

#### 1.1 Revisión de Modelos
- **Modelo OSI:**  
  | Capa               | Función                                                                 |
  |--------------------|-------------------------------------------------------------------------|
  | Física             | Encargado de la transmisión real de bits a través de medios físicos. Tanto el cálculo de la fórmula de Shannon, las técnicas de modulación, el wifi y los cables ethernet del proyecto pertenecen a esta capa.                   |
  | Enlace de Datos    | Colabora la transmisión libre de errores entre nodos, gestiona el direccionamiento a nivel de hardware. En el proyecto representa las direcciones MAC y el control de acceso al medio.                           |
  | Red                | Distribuye el direccionamiento lógico y la toma de decisiones de enrutamiento, por ejemplo, el subneteo y esquema de rutas. En el proyecto esta capa se encarga de la direccion de broadcast, rango de host, Enrutamiento con Dijkstra, routers y configuraciones IP                                 |
  | Transporte         |Se encarga de ejecutar una entrega confiable o en tiempo real,Usaremos  TCP + SFTP para la transferencia de archivos y UDP + RTP para el streaming.                                                 |
  | Sesión             |Establece, administra y finaliza conexiones entre aplicaciones, para la gestión de diálogos entre diferentes sistemas. La multiplexación forma parte de esta capa                                                         |
  | Presentación       |Representa y codifica los datos, haciendo el cifrado, compresión y transformación de diferentes formatos.                                                  |
  | Aplicación         | Se comunica directamente con los programas que utiliza el usuario, por ejemplo, protocolos FTP/SFTP para transferir archivos, HTTP/HTTPS para los de multimedia, etc...                                   |

- **Modelo TCP/IP:**  
  | Capa               | Equivalencia OSI                                                       |
  |--------------------|-------------------------------------------------------------------------|
  | Acceso a Red       | Sería igual que la capa física y enlace de datos en el modelo OSI. Se encarga de la transmisión de datos a nivel de hardware y de la comunican por medio físicos como el ethernet.                                              |
  | Internet           | Se asemeja a la capa de red del modelo OSI. En este se gestiona las direcciones IP y el enrutamiento, por ejemplo, el algoritmo de DIjkstra.                                                                   |
  | Transporte         | Parecida a la capa de transporte del modelo OSI. Gestiona los servicios de transporte ya sea confiable o sin conexión, TCP y UDP.                                                            |
  | Aplicación         | Equivalen a las capas 5, 6 y 7 del modelo OSI. Protocolos de trasnferencia, FTP y SFTP, manejo de contenidos HTTP y HTTPS, etc..                                    |

---

### 2. Capa Física – Capacidad y Modulación

#### 2.1 Capacidad
- **Fórmula de Shannon:**  
  \( C = B \log_2(1 + \text{SNR}) \)  
  - \( B = 300 \, \text{MHz} \), \( \text{SNR} = 20 \, \text{dB} \).  
  - **Resultado:** \( C \approx 1.997 \, \text{Gbps} \).

#### 2.2 Técnicas de Modulación
- **Cableado:** PAM16 (10GBase-T) para alta estabilidad.  
- **Inalámbrico:** OFDM con modulación adaptativa (QPSK a 256-QAM).  

---

### 3. Capa de Red – Subneteo y Enrutamiento

#### 3.1 Subneteo
- **Bloque IP:** `172.22.53.0/22` (1022 hosts/subred).  
- **Subredes:**  
  | Departamento  | Dirección de Red       | Rango de Hosts               |
  |---------------|------------------------|------------------------------|
  | Aulas         | 172.22.53.0            | 172.22.53.1 – 172.22.53.254  |
  | Laboratorios  | 172.22.54.0            | 172.22.54.1 – 172.22.54.254  |

#### 3.2 Enrutamiento
- **Algoritmo:** Dijkstra para rutas óptimas.  

---

### 4. Capa de Transporte – Selección y Cálculo de Ventana

#### 4.1 Protocolos
| Servicio               | Protocolo          | Justificación                              |
|------------------------|--------------------|--------------------------------------------|
| Transferencia de archivos | TCP + SFTP (puerto 22) | Confiabilidad y cifrado.                |
| Streaming              | UDP + RTP          | Baja latencia, tolerancia a pérdidas.     |

#### 4.2 Ventana Óptima
- **Cálculo:**  
  \( \text{Ventana} = \text{RTT} \times \text{Capacidad} = 0.05 \, \text{s} \times 249.625 \, \text{MB/s} \approx 12.5 \, \text{MB} \).  
- **Segmentos TCP:** 8542 (MSS = 1460 bytes).  

---

### 5. Capa de Aplicación – Servicios y Multiplexación

- **Transferencia de archivos:** SFTP (cifrado).  
- **Streaming:** HTTPS + MPEG-DASH (adaptativo).  
- **Multiplexación:** Hilos/procesos asíncronos.  

---

### 7. Seguridad – Estrategias y Configuraciones

#### 7.1 Medidas
1. **VPN:** Cifrado AES-256.  
2. **Firewalls (ACLs):** Filtrado por IP/protocolo.  
3. **NAT Dinámico:** Oculta IPs internas.  
4. **Cifrado:** AES-256 (simétrico) y RSA-2048 (asimétrico).  
5. **DNSSEC:** Protege contra spoofing.  

#### 7.2 Documentación
| Medida       | Función                          | Justificación                          |
|--------------|----------------------------------|----------------------------------------|
| VPN          | Conexión segura entre sedes      | Cifrado estándar, fácil mantenimiento. |
| ACLs         | Filtrado de tráfico              | Control de accesos.                    |

---

### 8. Implementación en Cisco Packet Tracer

#### 8.1 Topología
- Diseño de red con routers, switches y VLANs.  
#### 8.2 Pruebas
- Verificación de conectividad y QoS.  

--- 

**Nota:** Los detalles técnicos y cálculos están basados en el documento original.
