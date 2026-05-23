# embedded-system-broker-mqtt

> MQTT broker for asynchronous communication between embedded devices and backend services in the Embedded Hardware-Software System.  
> *Broker MQTT para la comunicación asíncrona entre dispositivos embebidos y servicios backend del Sistema Embebido Hardware-Software.*

---

## Language / Idioma

- [English Documentation](#en--english)
- [Documentación en Español](#es--español)

---

## EN | English

### Overview

This repository configures and deploys the MQTT broker that serves as the central messaging hub of the system. It decouples device-side telemetry publishing from backend data processing, enabling asynchronous and resilient communication across all system components.

### Role in the Ecosystem

The broker receives MQTT messages published by embedded devices and routes them to all registered subscribers, primarily the Core API. It enforces topic-based access control, connection management, and message persistence policies. No component communicates directly with a device; all device interaction is mediated through this broker.

### Technology Stack

| Component | Technology |
|:---|:---|
| Broker | Eclipse Mosquitto |
| Protocol | MQTT v3.1.1 / v5.0 |
| Deployment | Docker / Docker Compose |
| Authentication | Username/password, TLS certificates (optional) |

### Prerequisites

- Docker 24+ and Docker Compose v2+
- Open network ports: `1883` (MQTT), `8883` (MQTT over TLS), `9001` (WebSocket, optional)

### Project Structure

```
embedded-system-broker-mqtt/
├── config/
│   ├── mosquitto.conf     # Main broker configuration
│   └── passwd             # Credential file (generated, not committed)
├── data/                  # Persistent message store (volume)
├── log/                   # Broker logs (volume)
├── docker-compose.yml
└── README.md
```

### Environment Variables

| Variable | Description | Default |
|:---|:---|:---|
| `MQTT_PORT` | Plaintext MQTT port | `1883` |
| `MQTT_TLS_PORT` | TLS-secured MQTT port | `8883` |
| `MQTT_WS_PORT` | WebSocket port | `9001` |

### Setup and Deployment

```bash
# Start the broker
docker compose up -d

# View broker logs
docker compose logs -f

# Stop the broker
docker compose down
```

### Topic Structure

| Topic Pattern | Publisher | Subscriber |
|:---|:---|:---|
| `embedded/{device_id}/telemetry` | Device | Core API |
| `embedded/{device_id}/commands` | Core API | Device |
| `embedded/{device_id}/status` | Device | Core API |

---

## ES | Español

### Descripción General

Este repositorio configura y despliega el broker MQTT que actúa como concentrador central de mensajería del sistema. Desacopla la publicación de telemetría del lado del dispositivo del procesamiento de datos en el backend, habilitando una comunicación asíncrona y resiliente entre todos los componentes del sistema.

### Rol en el Ecosistema

El broker recibe los mensajes MQTT publicados por los dispositivos embebidos y los enruta hacia todos los suscriptores registrados, principalmente la API Core. Aplica políticas de control de acceso por tópico, gestión de conexiones y persistencia de mensajes. Ningún componente se comunica directamente con un dispositivo; toda interacción con dispositivos es mediada por este broker.

### Stack Tecnológico

| Componente | Tecnología |
|:---|:---|
| Broker | Eclipse Mosquitto |
| Protocolo | MQTT v3.1.1 / v5.0 |
| Despliegue | Docker / Docker Compose |
| Autenticación | Usuario/contraseña, certificados TLS (opcional) |

### Prerequisitos

- Docker 24+ y Docker Compose v2+
- Puertos de red abiertos: `1883` (MQTT), `8883` (MQTT sobre TLS), `9001` (WebSocket, opcional)

### Estructura del Proyecto

```
embedded-system-broker-mqtt/
├── config/
│   ├── mosquitto.conf     # Configuración principal del broker
│   └── passwd             # Archivo de credenciales (generado, no versionado)
├── data/                  # Almacén de mensajes persistentes (volumen)
├── log/                   # Logs del broker (volumen)
├── docker-compose.yml
└── README.md
```

### Variables de Entorno

| Variable | Descripción | Valor por defecto |
|:---|:---|:---|
| `MQTT_PORT` | Puerto MQTT en texto plano | `1883` |
| `MQTT_TLS_PORT` | Puerto MQTT con TLS | `8883` |
| `MQTT_WS_PORT` | Puerto WebSocket | `9001` |

### Configuración y Despliegue

```bash
# Iniciar el broker
docker compose up -d

# Ver logs del broker
docker compose logs -f

# Detener el broker
docker compose down
```

### Estructura de Tópicos

| Patrón de Tópico | Publicador | Suscriptor |
|:---|:---|:---|
| `embedded/{device_id}/telemetry` | Dispositivo | API Core |
| `embedded/{device_id}/commands` | API Core | Dispositivo |
| `embedded/{device_id}/status` | Dispositivo | API Core |
