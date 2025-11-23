


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

```
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

### Características Clave

1.  **Thread Safety con Java 24:** Se utiliza `ThreadLocal<WebDriver>` para garantizar que cada hilo de ejecución tenga su propia instancia del navegador, totalmente aislada de los demás hilos. Esto permite correr múltiples tests a la vez sin que se interfieran entre sí.
2.  **Configuración Dinámica:** El framework detecta automáticamente el entorno. Si detecta credenciales en las variables de entorno (`BS_USER`), ejecuta en la nube. Si no, ejecuta en local.
3.  **Hooks de Cucumber:** Gestión automática del ciclo de vida del driver (`@Before` para iniciar, `@After` para cerrar) optimizando recursos.

-----

## ⚡ Ejecución en Paralelo

La capacidad de paralelismo se controla mediante **TestNG** y el archivo `src/test/resources/testng.xml`.

  * **Nivel de Paralelismo:** Configurado en `parallel="methods"`.
  * **Límite de Hilos:** Controlado por `data-provider-thread-count`. Por defecto está configurado a **5** hilos simultáneos (ajustable según el plan de BrowserStack).

-----

## 🚀 Guía de Instalación y Ejecución

### Prerrequisitos

  * **Java JDK 24**: Debes tener instalada la versión 24 del JDK.
  * **Gradle**: El proyecto incluye el wrapper (`gradlew`), por lo que no necesitas instalarlo globalmente.

### 1\. Clonar el repositorio

```bash
git clone [https://github.com/TU_USUARIO/TU_REPO.git](https://github.com/TU_USUARIO/TU_REPO.git)
cd TU_REPO
```

### 2\. Ejecución Local (Chrome)

Para correr los tests en tu máquina local (ideal para depuración):

```bash
# En Windows
gradlew test

# En Mac/Linux
./gradlew test
```

*El sistema detectará la ausencia de credenciales de nube y levantará Chrome localmente.*

### 3\. Ejecución Remota (BrowserStack)

Para ejecutar la suite completa en la infraestructura de BrowserStack:

**Opción A: Configurar variables en el sistema**

```bash
export BS_USER="tu_usuario"
export BS_KEY="tu_access_key"
./gradlew test
```

**Opción B: Pasarlas en línea de comandos (Windows/Linux)**

```bash
BS_USER=tu_usuario BS_KEY=tu_clave ./gradlew test
```

-----

## 🤖 Integración Continua (CI/CD)

Este proyecto incluye un pipeline de **GitHub Actions** configurado en `.github/workflows/selenium-portfolio.yml`.

### Funcionamiento:

1.  **Trigger:** Cada vez que se hace un `push` a la rama `main`.
2.  **Entorno:** Levanta un contenedor Ubuntu con **Java 24**.
3.  **Seguridad:** Lee las credenciales desde **GitHub Secrets** (`BROWSERSTACK_USER` y `BROWSERSTACK_KEY`), inyectándolas en tiempo de ejecución.
4.  **Resultado:** Los tests se ejecutan en los servidores de BrowserStack y los resultados se reportan de vuelta a GitHub.

-----

## 📊 Reportes de Prueba

Tras la ejecución, el framework genera reportes detallados en HTML incluyendo capturas de pantalla (si se configura) y trazas de error.

Ubicación del reporte local:

> `target/cucumber/reports/overview-features.html`

-----

### 👤 Autor

**[Ronal]** - QA Automation Engineer

  * 💼 [LinkedIn]()
  * 📂 [Portfolio Web]()


```
```
