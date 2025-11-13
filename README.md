# PruebaPIX
Esta es el código realizado en PIX Studio para la prueba técnica

## 🤖 Proyecto: Automatización de Análisis de Productos (RPA con PIX Studio)

### 📄 Descripción del Proyecto

Este proyecto RPA, desarrollado con **PIX Studio** y siguiendo la **Plantilla Universal**, tiene como objetivo automatizar el análisis diario de productos de una tienda online ficticia, integrando diversas tecnologías.

El proceso maneja un flujo de datos de extremo a extremo: desde la obtención de datos de una **API pública** hasta el almacenamiento en una **base de datos**, la generación de un **reporte estadístico** y la entrega final a través de la **automatización web** y el almacenamiento en la nube.

**Tareas principales ejecutadas por el robot:**

1.  **Obtención y Respaldo de Datos:** Consume la API de Fake Store para obtener la lista de productos y guarda un archivo de respaldo en formato JSON.
2.  **Almacenamiento Persistente:** Almacena los productos en una Base de Datos (ej: SQLite), aplicando validación para evitar duplicados.
3.  **Generación de Reporte:** Genera un archivo `.xlsx` con estadísticas clave (precio promedio, conteo por categoría, etc.).
4.  **Integración con Microsoft Graph (OneDrive):** Sube el archivo JSON de respaldo y el reporte Excel a OneDrive de forma **desatendida** utilizando el flujo de **Refresh Token**.
5.  **Automatización Web:** Rellena y sube el reporte generado a un formulario web, registrando una evidencia de la confirmación.

---

## 🚀 Pasos para la Ejecución

El proceso se ejecuta en dos modos principales:

### **Modo 1: Primera Ejecución (Requiere Intervención Manual)**

Este modo es necesario solo la primera vez para obtener el `refresh_token` que permite la ejecución desatendida futura.

1.  **Inicialización (Init):** El robot carga las credenciales de configuración (Client ID, Client Secret) y la cadena de conexión a la DB.
2.  **Autenticación (Flujo de Dispositivo):** El robot inicia la solicitud POST a `/devicecode` (Paso 1), obtiene el `user_code` y la `verification_uri`.
3.  **Intervención Manual:** El usuario debe **ingresar el código de dispositivo** en el navegador en un plazo de 15 minutos.
4.  **Obtención del Token (Paso 3):** El robot realiza la solicitud POST a `/token` y extrae tanto el **`access_token`** como el **`refresh_token`** de la respuesta JSON.
5.  **Guardar `refresh_token`:** El robot guarda el `refresh_token` en un archivo local (ej: `config.json`) para su uso futuro.
6.  **Flujo de Proceso Continuo:** El robot continúa con las tareas de API, DB, Excel y Web.

### **Modo 2: Ejecuciones Posteriores (Totalmente Desatendidas)**

1.  **Inicialización (Init):** El robot carga las credenciales y el **`refresh_token`** guardado.
2.  **Autenticación Desatendida:** El robot omite los pasos manuales e inicia el flujo de **Refresh Token** (POST a `/token` con `grant_type=refresh_token`).
3.  **Obtención de Token:** Si el token de refresco es válido, Azure devuelve un nuevo `access_token` y un nuevo `refresh_token` (que debe sobrescribirse para mantener la persistencia).
4.  **Flujo de Proceso Continuo:** El robot continúa con las tareas de API, DB, Excel y Web, sin intervención humana.

---

## 🛠️ Requisitos o Dependencias

### Requisitos de Software

* **PIX RPA Studio:** Versión requerida para la ejecución del código fuente.
* **Microsoft Excel:** Necesario para la generación y guardado del reporte (`Reporte_YYYY-MM-DD.xlsx`).
* **Base de Datos:** Instancia de **SQLite** (o la DB seleccionada: PostgreSQL/SQL Server) con la tabla `Productos` inicializada.

### Credenciales y Configuraciones API

Todos los valores deben estar centralizados en un archivo de configuración (`config.json` o similar) y ser leídos al inicio del proceso.

| Configuración | Valor | Uso |
| :--- | :--- | :--- |
| **`Client ID`** | `91f0a712-1fd8-405d-ac57-a93565c2cf96` | Identificador de la aplicación de Azure. |
| **`Client Secret`** | (Valor confidencial) | Secreto de la aplicación, esencial para la autenticación desatendida. |
| **`Refresh Token`** | (Valor guardado) | Token de persistencia; necesario para las ejecuciones desatendidas. |
| **`DB Connection String`** | (Ej: `Data Source=./Datos/Productos.db`) | Cadena de conexión a la Base de Datos. |
| **`OneDrive Root Path`** | `/RPA/` | Ruta base en OneDrive para subir los archivos. |

---

## 🔗 Enlace del Formulario Usado

Para la **Automatización Web (Paso 4)**, se utilizó la siguiente URL del formulario web creado con Google Forms (o el servicio seleccionado).

**Enlace del Formulario:**

> `[PLACEHOLDER: Insertar aquí el enlace del Google Form/JotForm creado]`

**Campos del Formulario Requeridos:**

* Nombre del colaborador (Texto)
* Fecha de generación del reporte (Fecha)
* Comentarios (Opcional, Texto)
* **Subida de archivo** (El robot subirá `Reporte_YYYY-MM-DD.xlsx`).
