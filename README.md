# SentinelCloud

*Read this in other languages: [ENG](#ENG) | [ESP](#ESP)*

---

## ENG

An automated active defense and network security system designed to enforce dynamic micro-segmentation for IoT devices within cloud environments. Developed as a Final Graduation Project (Grade: 8.0/10).

## Project Overview
Device insecurity in corporate IoT environments represents a critical attack vector. **SentinelCloud** mitigates this risk by continuously monitoring network logs and executing automated quarantine protocols via Python when anomalous behavior or a known threat signature is detected.

## Team & Contributions
This project was developed as a collaborative graduation thesis. My core responsibilities and technical ownership included:
*   **Cloud Infrastructure Engineering:** Designing and deploying the AWS network architecture.
*   **Perimeter Security & Hardening:** Creating and managing dynamic Security Groups, IAM Roles, and access control policies to guarantee isolation.

## Architecture & Flow
`Compromised IoT Device` ──> `Anomalous Traffic/Log` ──> `Python Detection Script` ──> `AWS API Call (Boto3)` ──> `Security Group Revocation` ──> `Isolation`

### Core Features:
*   **Automated Triage:** Real-time analysis of simulated network traffic logs.
*   **Active Defense:** Immediate automated isolation of compromised hosts by updating AWS Security Groups and ACLs in <5 seconds.
*   **Perimeter Hardening:** Implements strict least-privilege access rules dynamically.

## Tech Stack
*   **Language:** Python 3.12
*   **Cloud Infrastructure:** Amazon Web Services (AWS VPC, Security Groups, IAM Roles)
*   **SDK:** Boto3 (AWS SDK for Python)

## Core Code Highlight
Below is the core Python logic utilizing the `boto3` library to isolate an instance by revoking its network access permissions upon alert triggering:

```python
import boto3

def isolate_compromised_host(instance_id, security_group_id):
    """
    Revokes all ingress traffic for a specific Security Group to isolate the host.
    """
    ec2 = boto3.client('ec2', region_name='eu-west-3') # Madrid Region
    try:
        response = ec2.revoke_security_group_ingress(
            GroupId=security_group_id,
            IpPermissions=[
                {
                    'IpProtocol': '-1', # All protocols
                    'FromPort': -1,
                    'ToPort': -1,
                    'IpRanges': [{'CidrIp': '0.0.0.0/0'}]
                }
            ]
        )
        print(f"[!] SUCCESS: Instance {instance_id} has been isolated.")
        return response
    except Exception as e:
        print(f"[-] ERROR isolating instance: {e}")
```

## ESP

La falta de seguridad en los dispositivos IoT en entornos corporativos representa un vector de ataque crítico. SentinelCloud mitiga este riesgo mediante la monitorización continua de logs de red y la ejecución de protocolos de cuarentena automatizados con Python cuando se detecta un comportamiento anómalo o una firma de amenaza conocida.

## Descripción del Proyecto
La falta de seguridad en los dispositivos IoT en entornos corporativos representa un vector de ataque crítico. SentinelCloud mitiga este riesgo mediante la monitorización continua de logs de red y la ejecución de protocolos de cuarentena automatizados con Python cuando se detecta un comportamiento anómalo o una firma de amenaza conocida.

## Equipo y Contribuciones
Este proyecto fue desarrollado de forma colaborativa. Mis responsabilidades principales y autoría técnica incluyeron:
*   **Ingeniería de Infraestructura Cloud:** Diseño y despliegue de la arquitectura de red en AWS.
*   **Seguridad Perimetral y Hardening:** Creación y gestión de Security Groups dinámicos, Roles de IAM y políticas de control de acceso para garantizar el aislamiento.

## Arquitectura y Flujo
`Dispositivo IoT Comprometido` ──> `Tráfico/Log Anómalo` ──> `Script de Detección en Python` ──> `Llamada API de AWS (Boto3)` ──> `Revocación de Security Group` ──> `Aislamiento`

### Características Principales:
*   **Triaje Automatizado:** Análisis en tiempo real de logs simulados de tráfico de red.
*   **Defensa Activa:** Aislamiento automatizado e inmediato de hosts comprometidos mediante la actualización de Security Groups y ACLs de AWS en menos de 5 segundos.
*   **Hardening Perimetral:** Implementación dinámica de reglas estrictas de mínimo privilegio.

## Tecnologías Utilizadas
*   **Lenguaje:** Python 3.12
*   **Infraestructura Cloud:** Amazon Web Services (AWS VPC, Security Groups, IAM Roles)
*   **SDK:** Boto3 (AWS SDK para Python)

## Código Destacado
A continuación se muestra la lógica central en Python que utiliza la librería `boto3` para aislar una instancia revocando sus permisos de acceso a la red cuando se activa una alerta:

```python
import boto3

def isolate_compromised_host(instance_id, security_group_id):
    """
    Revoca todo el tráfico de entrada para un Security Group específico para aislar el host.
    """
    ec2 = boto3.client('ec2', region_name='eu-west-3') # Región de Madrid
    try:
        response = ec2.revoke_security_group_ingress(
            GroupId=security_group_id,
            IpPermissions=[
                {
                    'IpProtocol': '-1', # Todos los protocolos
                    'FromPort': -1,
                    'ToPort': -1,
                    'IpRanges': [{'CidrIp': '0.0.0.0/0'}]
                }
            ]
        )
        print(f"[!] ÉXITO: La instancia {instance_id} ha sido aislada.")
        return response
    except Exception as e:
        print(f"[-] ERROR al aislar la instancia: {e}")
```
