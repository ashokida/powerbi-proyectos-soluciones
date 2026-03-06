# Problemas y soluciones para ejecutar un reporte de Power BI en Report Server

## 🚀 Contexto de pruebas:

En mi PC instale el Power Bi Desktop RS (versión Septiembre 2024)
En mi PC instale el Report Server (versión Septiembre 2024)
Instale el motor de base de datos MSSQL para alojar la base de datos de Report Server
La base de datos utilizada como origen de datos para el reporte está instalada en un servidor fuera del dominio (No utilizo Analysis Services)


### 💻 Entorno de la Solución
* [cite_start]**Versión de Software:** Power BI Desktop RS y Report Server (Septiembre 2024)[cite: 3, 4].
* [cite_start]**Servidor de Reportes:** `B1842ZACW0168` (Dominio: `CORREO`)[cite: 9, 10].
* [cite_start]**Origen de Datos:** SQL Server externo (`10.1.12.232`) con base de datos `Escrutinio_php`[cite: 12, 13].
* [cite_start]**Método de acceso:** DirectQuery mediante usuario SQL `qsense_reader`[cite: 14, 22].

### ❌ El Problema
[cite_start]Al intentar visualizar el reporte `TableroSiepPBIDesktopRS.pbix` desde el portal web, se presentaba el siguiente error técnico[cite: 15, 54]:

> [cite_start]*"No se pudo establecer una conexión con el servidor de Analysis Services. Asegúrese de que la cadena de conexión que ha escrito es correcta."* [cite: 54]

### ✅ Solución Aplicada
[cite_start]Para que el reporte funcione correctamente y sea accesible para otros usuarios, se deben realizar los siguientes pasos[cite: 59]:

1. **Configurar Variable de Entorno:**
   [cite_start]En el servidor de PBIRS, crear la variable de sistema `PBI_SQL_TRUSTED_SERVERS` e incluir la IP del servidor de datos (`10.1.12.232`) para habilitar la confianza entre servidores[cite: 60, 61].

2. **Permisos de Acceso al Portal:**
   [cite_start]En `Configuración del sitio` > `Seguridad`, agregar a los usuarios de dominio (NT) que requieren entrar al portal web[cite: 63].

3. **Seguridad del Reporte:**
   [cite_start]Asignar permisos específicos a los usuarios de dominio dentro de la configuración de seguridad del informe[cite: 64].

4. **Credenciales del Origen de Datos:**
   [cite_start]Configurar las credenciales de tipo "Autenticación básica" en las opciones del reporte dentro del portal para asegurar la conexión persistente a la base de datos[cite: 65, 50].

---

## 🛠️ Configuración de Referencia (Config Manager)
[cite_start]Detalles de la instancia utilizada para esta solución[cite: 23]:
* [cite_start]**Servicio Web:** `http://B1842ZACW0168/ReportServer`[cite: 34].
* [cite_start]**Portal Web:** `http://B1842ZACW0168/Reports`[cite: 44].
* [cite_start]**Base de Datos:** `ReportServer` (Modo Nativo)[cite: 37, 38].
* [cite_start]**Cuenta de Servicio:** Cuenta de servicio Virtual[cite: 28].

---
