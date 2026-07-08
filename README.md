# 🎮 IPS-Project-Dinosaur-Exploder (Sprint 1)

## 🛠️ Modificaciones y Mejoras Realizadas (Sprint 1)

Durante este sprint, nos enfocamos en estabilizar el entorno de desarrollo, implementar estándares estrictos de calidad de código y automatizar la entrega de métricas mediante GitHub Actions.

### 1. Backend & Arquitectura (Java + Maven)
* **Compatibilidad de Entorno:** Se estandarizó el proyecto para utilizar **Java 21** y **Maven**, asegurando la consistencia entre los entornos locales y los servidores de GitHub CI.
* **Métricas de Cobertura:** Se integró el plugin de **JaCoCo (Java Code Coverage)** en el ciclo de vida de Maven (`pom.xml`) para auditar la calidad de las pruebas unitarias de la versión de escritorio.

### 2. Automatización y Analítica (Python)
* **Módulo de Gestión de Métricas:** Se añadió un script en Python (`.py`) encargado de consumir la API de GitHub para calcular de forma dinámica las métricas del equipo (puntos de historia, commits y issues).
* **Visualización:** El script genera automáticamente un **Burndown Chart** (gráfico de trabajo pendiente) exportado como HTML/SVG estático.
* **Gestión de Dependencias:** Se configuraron las librerías analíticas (`matplotlib`, `requests`, `python-dateutil`) aislando el entorno de ejecución para cumplir con las normativas modernas de seguridad en Linux (PEP 668).

### 3. Pipeline de Integración Continua (CI)
Se diseñó un flujo automatizado en `.github/workflows/ci.yml` que se dispara ante cualquier `push` o `pull_request`:
* **Fase de Compilación:** Limpieza y construcción rápida con `mvn clean compile`.
* **Fase de Pruebas:** Ejecución de pruebas unitarias (`mvn test`) configuradas en modo *headless* (`java.awt.headless=true`) para entornos de servidor (Ubuntu).
* **Calidad de Código:** * Validación estricta de formato estético con **Spotless**.
  * Análisis estático de vulnerabilidades y malas prácticas (*code smells*) con **PMD**, optimizando las reglas para no interferir con código ajeno.

### 4. Pipeline de Despliegue Continuo (CD)
Se implementó un flujo de publicación automática en `.github/workflows/cd-pages.yml` exclusivo para la rama `main`:
* **Generación de Reportes:** Compila el proyecto, genera el reporte visual de JaCoCo y ejecuta de forma transparente el script analítico de Python.
* **Página Puente (Fallback):** Se diseñó un mecanismo de seguridad en Bash que inyecta un `index.html` de emergencia en caso de fallas de rutas del linter, garantizando que el sitio web de **GitHub Pages** nunca retorne un error 404.
* **Publicación:** Despliega de manera segura los resultados en la nube mediante autenticación OIDC (`id-token: write`).

---

## 📊 Resumen de la Infraestructura de Calidad

A continuación se detallan las herramientas y los comandos clave configurados en el proyecto:

| Componente / Herramienta | Función Principal | Comando Local de Validación |
| :--- | :--- | :--- |
| **JUnit 5 / Surefire** | Ejecución de pruebas unitarias en el backend. | `mvn test` |
| **PMD Plugin** | Detector de malas prácticas y deuda técnica. | `mvn pmd:check` |
| **Python 3 (`scripts/`)** | Generador del Burndown Chart del Sprint. | `python3 scripts/generate_burndown.py` |
| **GitHub Pages** | Servidor de hosting para los reportes de avance del equipo. | *Automatizado en la nube* |

---

## 🚀 Cómo Ejecutar el Proyecto Localmente

### Requisitos Previos
* **Java:** JDK 21 instalado.
* **Maven:** Configurado en las variables de entorno.
* **Python:** Versión 3.11 o superior.