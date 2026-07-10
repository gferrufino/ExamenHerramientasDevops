# 🐾 Tienda de Perritos - Despliegue Multi-capa en AWS EKS mediante CI/CD

Este repositorio contiene la arquitectura, el código fuente y las configuraciones de infraestructura como código necesarias para desplegar de forma automatizada la aplicación multi-capa **"Tienda de Perritos"** en la nube de Amazon Web Services (AWS) utilizando **Amazon EKS (Elastic Kubernetes Service)** y un pipeline de **CI/CD con GitHub Actions**.

El proyecto está diseñado bajo un enfoque de **microservicios**, garantizando la inmutabilidad de la infraestructura mediante contenedores Docker y la automatización extrema bajo principios de GitOps.

---

## 🏗️ Arquitectura de la Solución

La arquitectura de la aplicación en la nube se divide en tres capas independientes interconectadas, implementando el marco de buena arquitectura de AWS (*Well-Architected Framework*):

*   **Capa de Red (VPC):** Los recursos se alojan dentro de una VPC personalizada. Los nodos de cómputo residen en subredes privadas aisladas para mitigar la superficie de ataque, mientras que el tráfico externo se gestiona mediante componentes públicos.
*   **Capa de Cómputo y Orquestación (Amazon EKS):** Un clúster gestionado de Kubernetes distribuye los contenedores en un grupo de nodos compuesto por instancias EC2 `t3.large`.
*   **Registro de Contenedores (Amazon ECR):** Almacenamiento privado y seguro de las imágenes Docker del sistema firmadas por versión.
*   **Exposición del Servicio:** Un servicio de tipo `LoadBalancer` en Kubernetes aprovisiona de forma dinámica un balanceador de carga físico en AWS, otorgando un endpoint DNS público para los usuarios finales.

---

## 📂 Estructura del Proyecto

El repositorio está organizado bajo una estructura limpia y estandarizada:

```text
├── .github/
│   └── workflows/
│       └── deploy.yml      # Definición del Pipeline de CI/CD (GitHub Actions)
├── k8s/
│   ├── mysql.yaml         # Manifiesto declarativo de la Base de Datos (Deployment + Service)
│   ├── backend.yaml       # Manifiesto declarativo de la API (Deployment + Service)
│   └── frontend.yaml      # Manifiesto declarativo de la Interfaz Web (Deployment + Service + LoadBalancer)
├── frontend/              # Código fuente de la interfaz de usuario y su Dockerfile
├── backend/               # Código fuente de la lógica de negocio y su Dockerfile
└── mysql/                 # Configuraciones iniciales de la base de datos y su Dockerfile