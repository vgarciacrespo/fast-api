# 🚗 API de Gestión de Coches con FastAPI y SQLAlchemy Async

¡Bienvenido al proyecto **Gestión de Coches**! Esta es una API RESTful moderna, rápida y de alto rendimiento diseñada con **FastAPI** y utilizando **SQLAlchemy** de manera totalmente asíncrona para interactuar con una base de datos **SQLite** (a través de `aiosqlite`).

El objetivo principal de este servicio es proporcionar un sistema robusto para la creación, lectura, actualización y eliminación (CRUD) de registros de vehículos asociados a diferentes clientes.

---

## 🛠️ Tecnologías y Herramientas

El proyecto está construido sobre un stack de tecnologías modernas del ecosistema Python:

*   **[FastAPI](https://fastapi.tiangolo.com/):** Framework web moderno de alto rendimiento para construir APIs con Python 3.8+.
*   **[SQLAlchemy (Async)](https://www.sqlalchemy.org/):** El ORM SQL más potente de Python, configurado con soporte asíncrono completo.
*   **[aiosqlite](https://github.com/omnilib/aiosqlite):** Driver asíncrono para SQLite, permitiendo operaciones I/O no bloqueantes en la base de datos.
*   **[Pydantic](https://docs.pydantic.dev/):** Validación de datos y modelado de esquemas mediante tipos estándar de Python.
*   **[Uvicorn](https://www.uvicorn.org/):** Servidor ASGI ultrarrápido para ejecutar la aplicación.

---

## 📂 Estructura del Proyecto

El proyecto está organizado de manera modular y limpia en el directorio `mi_api`:

```bash
fast-api/
├── mi_api/
│   ├── database.py       # Configuración del motor asíncrono y sesión de SQLAlchemy
│   ├── dependencies.py   # Dependencias inyectables (proveedor de sesión DB)
│   ├── init_db.py        # Script para inicializar/crear las tablas de la base de datos
│   ├── main.py           # Inicialización de la aplicación FastAPI, esquemas Pydantic y Endpoints
│   ├── models.py         # Definición de los modelos ORM (entidad Coche)
│   └── mi_api.db         # Archivo de base de datos SQLite (generado automáticamente)
├── .gitignore            # Archivos y carpetas omitidas en el control de versiones
└── README.md             # Documentación general del proyecto (este archivo)
```

---

## 📊 Arquitectura y Modelos de Datos

### Entidad `Coche` (`models.py`)
El modelo de datos almacena la información esencial de cada vehículo en una tabla de SQLite llamada `coches`:

| Campo | Tipo | Restricciones / Índices | Descripción |
| :--- | :--- | :--- | :--- |
| `id` | `Integer` | Clave Primaria, Autoincremental, Indexado | Identificador único del coche. |
| `cliente_id` | `Integer` | Indexado | ID del cliente propietario del vehículo. |
| `marca` | `String` | Indexado | Marca del fabricante (ej: Toyota, Ford). |
| `modelo` | `String` | Indexado | Modelo específico (ej: Corolla, Mustang). |
| `matricula` | `String` | Único, Indexado | Placa o patente de identificación única. |

---

## 🚀 Instalación y Configuración

Sigue estos sencillos pasos para configurar y ejecutar el proyecto localmente:

### 1. Clonar o acceder al proyecto
Asegúrate de estar en el directorio raíz del proyecto:
```bash
cd fast-api
```

### 2. Crear e iniciar el entorno virtual (Recomendado)
En macOS/Linux:
```bash
python3 -m venv venv
source venv/bin/activate
```
En Windows:
```bash
python -m venv venv
venv\Scripts\activate
```

### 3. Instalar las dependencias
Instala los paquetes necesarios utilizando `pip`:
```bash
pip install fastapi uvicorn sqlalchemy aiosqlite pydantic
```

### 4. Inicializar la Base de Datos
Para crear la base de datos SQLite (`mi_api.db`) y la tabla de coches basándose en los modelos ORM, ejecuta el script de inicialización:
```bash
python mi_api/init_db.py
```

---

## 💻 Ejecución del Servidor

Para levantar el servidor web de desarrollo con recarga automática (*auto-reload*), ejecuta el siguiente comando desde la carpeta `mi_api`:

```bash
cd mi_api
uvicorn main:app --reload
```

El servidor estará disponible en: **`http://127.0.0.1:8000`**

---

## 📖 Documentación Interactiva

FastAPI genera automáticamente documentación interactiva y profesional para todos tus endpoints. Con el servidor corriendo, puedes acceder a ella a través de:

*   **Interactive Docs (Swagger UI):** [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
    > Excelente interfaz visual para probar los endpoints directamente desde tu navegador.
*   **Alternative Docs (ReDoc):** [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)
    > Documentación limpia, esquematizada y optimizada para su lectura.

---

## 📡 Detalle de Endpoints (API Reference)

La API cuenta con las siguientes rutas de servicio:

### 🏠 General

#### `GET /`
Retorna un mensaje de bienvenida de cortesía.
*   **Ejemplo de Respuesta:**
    ```json
    {
      "mensaje": "¡Bienvenido a FastAPI!"
    }
    ```

---

### 🚗 CRUD de Coches (Ruta Base: `/coches`)

#### Esquema de Entrada / Creación (`CocheCreate`):
```json
{
  "cliente_id": 101,
  "marca": "Hyundai",
  "modelo": "Tucson",
  "matricula": "1234-XYZ"
}
```

---

#### ➕ `POST /coches/`
Registra un nuevo coche en la base de datos.
*   **Parámetros:** Cuerpo en formato JSON (`CocheCreate`).
*   **Respuesta Exitosa (200 OK):**
    ```json
    {
      "id": 1,
      "cliente_id": 101,
      "marca": "Hyundai",
      "modelo": "Tucson",
      "matricula": "1234-XYZ"
    }
    ```

---

#### 📋 `GET /coches/`
Obtiene la lista completa de todos los coches registrados.
*   **Respuesta Exitosa (200 OK):**
    ```json
    [
      {
        "id": 1,
        "cliente_id": 101,
        "marca": "Hyundai",
        "modelo": "Tucson",
        "matricula": "1234-XYZ"
      }
    ]
    ```

---

#### 🔍 `GET /coches/{coche_id}`
Obtiene los detalles de un coche específico usando su ID único.
*   **Parámetros:** `coche_id` (en la URL).
*   **Respuesta si existe (200 OK):**
    ```json
    {
      "id": 1,
      "cliente_id": 101,
      "marca": "Hyundai",
      "modelo": "Tucson",
      "matricula": "1234-XYZ"
    }
    ```
*   **Respuesta si no existe (404 Not Found):**
    ```json
    {
      "detail": "Coche no encontrado"
    }
    ```

---

#### ✏️ `PUT /coches/{coche_id}`
Actualiza los datos de un coche existente.
*   **Parámetros:** `coche_id` (en la URL) y cuerpo JSON con los nuevos datos (`CocheCreate`).
*   **Respuesta Exitosa (200 OK):**
    ```json
    {
      "id": 1,
      "cliente_id": 101,
      "marca": "Hyundai",
      "modelo": "Tucson",
      "matricula": "9999-ABC"
    }
    ```
*   **Respuesta si no existe (404 Not Found):**
    ```json
    {
      "detail": "Coche no encontrado"
    }
    ```

---

#### ❌ `DELETE /coches/{coche_id}`
Elimina un coche de la base de datos.
*   **Parámetros:** `coche_id` (en la URL).
*   **Respuesta Exitosa (200 OK):**
    ```json
    {
      "mensaje": "Coche eliminado"
    }
    ```
*   **Respuesta si no existe (404 Not Found):**
    ```json
    {
      "detail": "Coche no encontrado"
    }
    ```

---

## ⚡ Patrones Asíncronos Implementados
A diferencia de los enfoques tradicionales síncronos, este proyecto aprovecha al máximo la concurrencia nativa de Python mediante:
1.  **Endpoints asíncronos (`async def`):** Liberan el hilo del servidor mientras esperan operaciones de red o de base de datos.
2.  **SQLAlchemy Async Engine (`create_async_engine`):** Ejecuta consultas SQL sin bloquear el bucle de eventos principal.
3.  **Generadores asíncronos (`yield` en `dependencies.py`):** Asegura que la sesión de la base de datos se abra y cierre correctamente y de manera asíncrona para cada solicitud del cliente.

---

## 📝 Licencia
Este proyecto es de código abierto. Siéntete libre de utilizarlo, modificarlo y adaptarlo a tus necesidades académicas o profesionales.
