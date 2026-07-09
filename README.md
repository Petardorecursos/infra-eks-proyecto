# infra-eks-proyecto

# 🚀 Plataforma Innovatech Chile - Infraestructura y Orquestación (AWS EKS)

Este repositorio actúa como el **núcleo unificador y única fuente de verdad (Single Source of Truth)** para la arquitectura Cloud Native de la plataforma de ventas y despachos de Innovatech. 

En lugar de contener código de aplicación, este repositorio centraliza el gobierno de la infraestructura (IaC), los manifiestos de Kubernetes, la orquestación local con Docker Compose y la documentación general de la arquitectura DevOps.

---

## 🏗️ Ecosistema de Microservicios

El proyecto está diseñado bajo una arquitectura desacoplada. El código fuente de las aplicaciones se mantiene y versiona de forma independiente en los siguientes repositorios:

* 🌐 **Frontend (Gestión de Despachos - React):** [Petardorecursos/front-despacho](https://github.com/Petardorecursos/front-despacho)
* ⚙️ **Backend (Módulo de Ventas - Spring Boot):** [Petardorecursos/back-ventas-springboot](https://github.com/Petardorecursos/back-ventas-springboot)
* 📦 **Backend (Módulo de Despachos - Spring Boot):** [Petardorecursos/back-despachos-springboot](https://github.com/Petardorecursos/back-despachos-springboot)

---

## ☁️ Arquitectura Cloud (Producción)

El entorno productivo está alojado íntegramente en **Amazon Web Services (AWS)**, aplicando principios de alta disponibilidad, tolerancia a fallos y mínimo privilegio:

1. **Capa de Red (VPC):** Segmentación estricta con subredes públicas (DMZ) para Balanceadores de Carga y subredes privadas aisladas para el cómputo.
2. **Capa de Orquestación (Amazon EKS):** Clúster Kubernetes gestionado que administra los pods de los microservicios. Implementa políticas de *Horizontal Pod Autoscaling (HPA)* para reaccionar dinámicamente ante picos de tráfico.
3. **Capa de Datos:** Base de datos relacional MySQL aprovisionada externamente a los pods para garantizar persistencia y seguridad de la información.
4. **Seguridad Perimetral:** Reglas restrictivas de *Security Groups* y roles *IAM* asegurando que los componentes solo se comuniquen a través de los puertos estrictamente necesarios.

---

## 🔄 Pipeline CI/CD (GitHub Actions)

La entrega de valor continua está automatizada a través de GitHub Actions en cada uno de los repositorios de código. El flujo estandarizado incluye:

* **Build & Test:** Validación y empaquetado de artefactos (Maven/Vite) utilizando Dockerfiles multietapa.
* **Push (Amazon ECR):** Las imágenes minimalistas son versionadas con doble etiquetado (`latest` y SHA del commit) y almacenadas en repositorios privados.
* **Deploy Automatizado:** El agente actualiza el manifiesto en el clúster EKS y ejecuta un *Rolling Update* (Zero Downtime) para disponibilizar la nueva versión de manera transparente al usuario final.
* **Gestión de Secretos:** Integración con GitHub Secrets para aislar llaves de AWS (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`).

---

## 💻 Entorno de Desarrollo Local

Para garantizar la paridad entre desarrollo y producción, este repositorio incluye un entorno orquestado con **Docker Compose**. Cualquier miembro del equipo puede levantar el ecosistema completo en su máquina local en cuestión de segundos.

### Prerrequisitos
* [Docker Desktop](https://www.docker.com/products/docker-desktop/)
* [AWS CLI v2](https://aws.amazon.com/cli/) (Autenticado con permisos de lectura para ECR)

### Instrucciones de Levantamiento

1. **Clonar este repositorio:**
   ```bash
   git clone [https://github.com/Petardorecursos/infra-eks-proyecto.git](https://github.com/Petardorecursos/infra-eks-proyecto.git)
   cd infra-eks-proyecto
