# Laboratorio de Seguridad de Redes - Práctica 1

**Estudiante:** Rafael Andrés Reyes  
**Matrícula:** 2025-0784  
**Asignatura:** Seguridad de Redes  
**Profesor:** Jonathan Rondon  

---

Video Demostrativo
https://itlaedudo-my.sharepoint.com/:v:/g/personal/20250784_itla_edu_do/IQA5-u2YmZirTKto9B5g4JJcAREUXlpajNjGkO2moBF76io?e=f9PYyO&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D


---

 Propósito del Laboratorio
El propósito de este laboratorio es diseñar e implementar una topología de red segura utilizando un firewall de próxima generación (Fortinet). Se busca segmentar la red en VLANs, aislar servidores críticos (Web y Base de Datos), e implementar controles avanzados de seguridad (DPI), incluyendo prevención de intrusiones (IPS) para bloquear inyecciones SQL, filtrado de archivos para prevenir descargas de malware (.exe), y políticas de mitigación de DoS.

---

## 🗺️ Diagrama de la Topología
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/734ce088-a630-4984-9899-7f2d2c57e834" />


---

## 🏗️ Direccionamiento IP (Basado en Matrícula: 784)
*   **VLAN 10 (Usuarios):** 10.7.84.0/25 (Gateway: 10.7.84.1)
*   **VLAN 20 (Servidores):** 10.7.84.128/28 (Gateway: 10.7.84.129)
*   **Web Server:** 10.7.84.130
*   **DB Server:** 10.7.84.131

---

## 🛡️ Configuraciones de Seguridad (Evidencias)

### 1. Políticas de Firewall (IPv4 Policies) y NAT
Se implementó una política para salida a internet con NAT, se permitió el tráfico HTTPS de Usuarios a Servidor Web, y se bloqueó explícitamente el acceso al Servidor DB.
<img width="1365" height="718" alt="image" src="https://github.com/user-attachments/assets/49cc5063-b25f-42c1-96bb-fc3df05c7958" />


### 2. Inspección Profunda (DPI) y Prevención de Inyección SQL
Se configuró un perfil de IPS asociado a un perfil de Deep Inspection SSL para mitigar ataques web.
*   **Intento de ataque (Payload):** `https://10.7.84.130/?id=1' OR '1'='1`
*   **Bloqueo y Cuarentena:**


### 3. Filtrado de Archivos (Bloqueo de .exe)
Se aplicó un File Filter en la política de salida a internet.
<img width="1352" height="197" alt="image" src="https://github.com/user-attachments/assets/ec102c26-c3c5-4c75-87f7-4fe11b84227c" />


### 4. Mitigación de DoS (Rate Limiting)
Se implementó una política IPv4 DoS limitando el `tcp_syn_flood` a 50 conexiones concurrentes para proteger el servidor Web.
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/f8569af2-c1b7-4fbe-aecf-09471a6e2689" />
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/4e0067b4-10df-49dd-b130-c954361cc9e4" />


---

## 💻 Scripts y Running-Configs
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/19035021-d6d1-4630-8994-c2fabe59615a" />
no me permitio subir el running config (lo mostrare en el video)

### 1. Script Netplan (Web Server - Ubuntu)
```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: no
      addresses:
        - 10.7.84.130/28
      gateway4: 10.7.84.129
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
