# PruebaPIX
Esta es el código realizado en PIX Studio para la prueba técnica

## 🤖 Proyecto: Automatización de Análisis de Productos (RPA con PIX Studio)

### 📄 Descripción del Proyecto

Este proyecto RPA, desarrollado con **PIX Studio** y siguiendo la **Plantilla Universal**, tiene como objetivo automatizar el análisis diario de productos de una tienda online ficticia, integrando diversas tecnologías.

El proceso maneja un flujo de datos de extremo a extremo: desde la obtención de datos de una **API pública** hasta el almacenamiento en una **base de datos**, la generación de un **reporte estadístico** y la entrega final a través de la **automatización web** y el almacenamiento en la nube.

**Tareas principales ejecutadas por el robot:**

1.  **Obtención y Respaldo de Datos:** Consume la API de Fake Store para obtener la lista de productos y guarda un archivo de respaldo en formato JSON.
2.  **Almacenamiento Persistente:** Almacena los productos en una Base de Datos, aplicando validación para evitar duplicados.
3.  **Generación de Reporte:** Genera un archivo `.xlsx` con estadísticas clave (precio promedio, conteo por categoría, precio promedio por categoria).
4.  **Integración con Microsoft Graph (OneDrive):** Sube el archivo JSON de respaldo y el reporte Excel a OneDrive de forma **desatendida** utilizando el flujo de **Refresh Token**.
5.  **Automatización Web:** Rellena y sube el reporte generado a un formulario web, registrando una evidencia de la confirmación.

---

## 🚀 Pasos para la Ejecución

1.  **Inicialización (Init):** El robot carga las credenciales de configuración (Client ID, Client Secret) y la cadena de conexión a la DB.
2.  **Guardar `refresh_token`:** El robot guarda el `refresh_token` en un archivo local para su uso futuro.
3.  **Flujo de Proceso Continuo:** El robot continúa con las tareas de API, DB, Excel y Web.
4.  **Autenticación Desatendida:** El robot inicia el flujo de **Refresh Token** (POST a `/token` con `grant_type=refresh_token`).

---

## 🛠️ Requisitos o Dependencias

### Requisitos de Software

* **PIX RPA Studio:** 2024.10.
* **Microsoft Excel:** Necesario para la generación y guardado del reporte (`Reporte_YYYY-MM-DD.xlsx`).
* **Base de Datos:** Instancia de **ACCESS**  con la tabla `Productos` inicializada.

### Credenciales y Configuraciones API

Todos los valores deben estar centralizados en un archivo de configuración (`config.xlsx`) y ser leídos al inicio del proceso.

| Configuración | Valor | Uso |
| :--- | :--- | :--- |
| **`Client ID`** | (Valor confidencial) | Identificador de la aplicación de Azure. |
| **`Client Secret`** | (Valor confidencial) | Secreto de la aplicación, esencial para la autenticación desatendida. |
| **`Refresh Token`** | (Valor guardado) | Token de persistencia; necesario para las ejecuciones desatendidas. |
| **`DB Connection String`** | \PruebaTecnica\Data\DatabasePIX.accdb | Cadena de conexión a la Base de Datos. |
| **`OneDrive Root Path`** | `/RPA/` | Ruta base en OneDrive para subir los archivos. |

---

## 🔗 Enlace del Formulario Usado

Para la **Automatización Web (Paso 4)**, se utilizó la siguiente URL del formulario web creado con Google Forms.

**Enlace:**

> `https://docs.google.com/forms/d/e/1FAIpQLSc8-AkwuO9U3nla7ogmsSq3walN_ZrieUlruJNQ_I2eO6MOrQ/viewform?usp=dialog`

**Campos del Formulario Requeridos:**

* Nombre del colaborador (Texto)
* Fecha de generación del reporte (Fecha)
* Comentarios (Opcional, Texto)
* **Subida de archivo** (El robot subirá `Reporte_YYYY-MM-DD.xlsx`).
