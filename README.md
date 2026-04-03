# 🚀 Windows Server 2022: RDS RemoteApp & Web Client (HTML5) Deployment

![Windows Server](https://img.shields.io/badge/Windows_Server-2022-0078D6?style=for-the-badge&logo=windows)
![IIS](https://img.shields.io/badge/IIS-Web_Server-blue?style=for-the-badge)
![PowerShell](https://img.shields.io/badge/PowerShell-Automation-5391FE?style=for-the-badge&logo=powershell)
![Cybersecurity](https://img.shields.io/badge/Cybersecurity-ITLA-red?style=for-the-badge&logo=security)
Link del video: https://youtu.be/ZwUoDQDAiSw

**Autor:** Martin Alexander Perez Moya  
**Matrícula:** 2024-2295  
**Institución:** Instituto Tecnológico de Las Américas (ITLA)  
**Programa:** Tecnólogo en Seguridad Informática  

---

## 📌 Objetivo del Proyecto
Implementar y asegurar un entorno de aplicaciones remotas utilizando los Servicios de Escritorio Remoto (RDS) en Windows Server 2022. El proyecto se centra en la publicación de aplicaciones nativas y un portal web corporativo personalizado (IIS) a través del **RD Web Client (HTML5)**, garantizando la confidencialidad e integridad de la conexión mediante la correcta implementación de certificados digitales (SSL/TLS) y resolución de conflictos de WebSockets.

---

## 🏗️ Topología y Entorno de Laboratorio

El entorno fue desplegado utilizando máquinas virtuales con la siguiente configuración de red:

| Dispositivo | Sistema Operativo | Rol Principal | Dirección IP |
| :--- | :--- | :--- | :--- |
| **Servidor Host** | Windows Server 2022 | DC, RDS Broker, IIS | `10.0.0.178/24` |
| **Estación Cliente**| Windows 10  | Cliente de Prueba | `10.0.0.179/24` |

* **Dominio Local:** `LAB.LOCAL`
* **FQDN del Servidor:** `WIN-27BOGTTJA16.LAB.LOCAL`

---

## ⚙️ Configuraciones Principales

### 1. Despliegue de Servicios (RDS)
Se configuró una **QuickSessionCollection** habilitando los siguientes roles de infraestructura:
* RD Connection Broker
* RD Web Access
* RD Session Host

### 2. Portal Corporativo (IIS)
Se personalizó el servicio de *Internet Information Services (IIS)* creando una página estática (`index.html`) que funciona como intranet corporativa. Esta página fue publicada como una *RemoteApp* utilizando Microsoft Edge, configurando los parámetros de línea de comandos para apuntar directamente al recurso web local.

### 3. Seguridad y Criptografía (Resolución de Conflictos TLS)
La fase más crítica del despliegue consistió en asegurar la confianza de la conexión HTML5. Se resolvieron los errores técnicos de negociación TLS (`GenericSecurityError 16` y `CertMismatch 7`):

* **Generación de Certificado:** Creación de un certificado con el propósito específico de *Server Authentication*.
* **Raíz de Confianza:** Importación del certificado en el almacén *Trusted Root Certification Authorities* tanto en el servidor como en el cliente para evitar advertencias de seguridad en el navegador.
* **Sincronización de WebSockets:** Vinculación manual de la huella criptográfica (Thumbprint) al Broker del RD Web Client mediante PowerShell para habilitar el tráfico seguro a través del puerto `3392`.

<details>
<summary>💻 Ver Comandos Clave y demostracion (PowerShell)</summary>

```powershell
# Importación del certificado al Broker para el RD Web Client (HTML5)
Import-Module RDWebClientManagement
Import-RDWebClientBrokerCert -Path "C:\Certs\webclient.cer"

# Publicación de la configuración y reinicio de servicios
Publish-RDWebClientPackage -Type Production -Latest
Restart-Service RDMS -Force
Restart-Service W3SVC -Force
```

#     Demostracion

## WEB CLIENT

<img width="741" height="496" alt="image" src="https://github.com/user-attachments/assets/f9b42ff6-bec5-456b-9301-49ee4ff88dd6" />


<img width="1517" height="830" alt="image" src="https://github.com/user-attachments/assets/f731a741-cb3e-4d62-9d0e-c368f5e96505" />


<img width="1064" height="745" alt="image" src="https://github.com/user-attachments/assets/d52c021c-2ef9-4b1b-a4d5-524398726343" />



## APP


  <img width="487" height="333" alt="image" src="https://github.com/user-attachments/assets/52e5d92d-ab0b-412f-9a03-3084fc544074" />

         

  <img width="502" height="398" alt="image" src="https://github.com/user-attachments/assets/1ee1fe68-1c43-4e8a-9966-2290d726c556" />



  <img width="1328" height="750" alt="image" src="https://github.com/user-attachments/assets/8f095848-2f9b-44fc-a19a-33cd723dbdb6" />


