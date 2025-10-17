# 🚀 Proyecto: Contenerización, Linting y Tests con Flask

Este proyecto demuestra una configuración robusta de desarrollo y despliegue para una pequeña aplicación **Flask** utilizando **Docker** y asegurando la calidad del código mediante **pre-commit hooks** de Git, que ejecutan *linting* (con **Black**) y *testing* (con **Pytest**).

---

## 🛠️ Cómo levantar la aplicación (Producción)

Para construir y ejecutar la versión final de la aplicación dentro de un contenedor Docker simple:

1.  **Construir la imagen:**
    ```bash
    docker build -t mi-app:1.0 .
    ```
2.  **Ejecutar el contenedor:**
    ```bash
    docker run --rm -p 8000:8000 mi-app:1.0
    ```
    La aplicación estará accesible en tu navegador en `http://localhost:8000`.

---

## 💻 Entorno de Desarrollo y Herramientas

Utilizamos un servicio de desarrollo (`dev`) definido en `docker-compose.yml` que monta el código localmente, permitiendo el uso de herramientas de *linting* y *testing* en un entorno aislado y consistente.

### Acceder al entorno de desarrollo (Shell)

Para interactuar directamente con la shell del contenedor de desarrollo (donde están instalados Black y Pytest):

1.  **Construir la imagen de desarrollo (si no se ha hecho):**
    ```bash
    docker compose build dev
    ```
2.  **Ejecutar un comando básico (ejemplo: verificar la versión de Python):**
    ```bash
    docker compose run --rm -T dev python --version
    ```

---

## ✅ Ejecución Manual de Tests y Formateo

Si deseas ejecutar las herramientas de calidad de forma manual y fuera del proceso de *commit*:

| Tarea | Comando de Docker Compose |
| :--- | :--- |
| **Ejecutar Tests** (Pytest) | `docker compose run --rm -T dev pytest` |
| **Verificar Formato** (Black) | `docker compose run --rm -T dev black --check .` |
| **Autoformatear Código** (Black) | `docker compose run --rm -T dev black .` |

---

## 🛡️ Git Pre-Commit Hook

Se ha configurado un **hook** de Git llamado `pre-commit` en `.git/hooks/pre-commit`.

### Cómo actúa:

El script de `pre-commit` se ejecuta **automáticamente antes de que se complete cualquier `git commit`**. Su propósito es garantizar que solo el código de alta calidad se suba al repositorio.

1.  **Verificación de Formato (Black):**
    * Ejecuta `black`. Si encuentra errores, **los corrige automáticamente** e inmediatamente **aborta el commit**. El desarrollador debe hacer un `git add .` para incluir las correcciones y reintentar el commit.
2.  **Ejecución de Tests (Pytest):**
    * Solo si el código está bien formateado, ejecuta `pytest`. Si **algún test falla**, el script **aborta el commit**.

Este mecanismo asegura que el repositorio principal siempre mantenga un código **formateado** y **funcional**.
