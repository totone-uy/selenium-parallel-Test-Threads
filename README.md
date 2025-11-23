# selenium-parallel-Test-Threads
# 🚀 Framework de Automatización de Pruebas E2E (Selenium + Java 24)

![Java](https://img.shields.io/badge/Java-24-orange?style=for-the-badge&logo=java)
![Selenium](https://img.shields.io/badge/Selenium-4.x-green?style=for-the-badge&logo=selenium)
![BrowserStack](https://img.shields.io/badge/BrowserStack-Cloud-blue?style=for-the-badge)
![CI/CD](https://img.shields.io/badge/GitHub_Actions-Enabled-black?style=for-the-badge&logo=github-actions)

Este repositorio contiene un framework de automatización de pruebas robusto, escalable y moderno, diseñado para validar aplicaciones web críticas. El proyecto está construido utilizando las últimas características de **Java 24** y sigue las mejores prácticas de la industria (Clean Code, SOLID, POM).

El objetivo principal de este portfolio es demostrar la capacidad de ejecutar pruebas concurrentes (paralelas) en una infraestructura en la nube (**BrowserStack**) integrada en un pipeline de **CI/CD**.

---

## 🛠️ Stack Tecnológico

La arquitectura del proyecto se basa en las siguientes herramientas:

* **Lenguaje:** [Java 24](https://jdk.java.net/24/) (Aprovechando las últimas mejoras de rendimiento y sintaxis).
* **Build Tool:** Gradle (Kotlin DSL) para una gestión de dependencias rápida y segura.
* **Core Automation:** Selenium WebDriver.
* **BDD Framework:** Cucumber (Gherkin) para pruebas legibles por negocio.
* **Test Runner:** TestNG (Configurado para orquestación paralela).
* **Patrón de Diseño:** Page Object Model (POM) + Factory Pattern.
* **Cloud Infrastructure:** BrowserStack Automate.
* **CI/CD:** GitHub Actions.
* **Reporting:** Extent Reports / Cucumber Reports.

---

## 🏗️ Arquitectura y Diseño

El framework ha sido diseñado para resolver el problema común de la "Condición de Carrera" (Race Condition) en ejecuciones paralelas.

### Diagrama de Flujo de Ejecución

```mermaid
graph TD
    CI[GitHub Actions / User] -->|Trigger| Gradle[Gradle Test Task]
    Gradle -->|Invoca| TestNG[TestNG Suite XML]
    TestNG -->|Distribuye Hilos| Runner[Test Runner]
    
    subgraph "Parallel Execution Pool (Java 24 Threads)"
        Runner -->|Thread 1| ScenarioA[Scenario: Login]
        Runner -->|Thread 2| ScenarioB[Scenario: Checkout]
        
        ScenarioA -->|Init| DriverFactory
        ScenarioB -->|Init| DriverFactory
        
        DriverFactory -->|Check Env| Env{¿Variables BS Existes?}
        
        Env -- Sí --> Remote[RemoteWebDriver (BrowserStack)]
        Env -- No --> Local[ChromeDriver Local]
        
        Remote -->|ThreadLocal| TL_A[Driver Aislado A]
        Remote -->|ThreadLocal| TL_B[Driver Aislado B]
    end
    
    TL_A --> CloudA[Nube BrowserStack - Sesión 1]
    TL_B --> CloudB[Nube BrowserStack - Sesión 2]
```
