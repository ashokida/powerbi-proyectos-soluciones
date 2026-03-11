# Problemas y soluciones para ejecutar un reporte de Power BI en Report Server

## 🚀 Contexto de pruebas:

* En mi PC instale el Power Bi Desktop RS (Instalador: PBIDesktopSetupRS_x64.exe Versión Septiembre 2024)
* En mi PC instale el Report Server (Instalador: PowerBIReportServer.exe - Versión Septiembre 2024. Deben coincidir version tanto Report Server como PowerBi Desktop RS)
* Instale el motor de base de datos MSSQL para alojar la base de datos de Report Server (Instalador: SQL2022-SSEI-Dev.exe)
* La base de datos utilizada como origen de datos para el reporte está instalada en un servidor fuera del dominio (No utilizo Analysis Services)

## 📘 Datos Relevantes:
* Entorno: Windows
* El nombre de mi PC: ""
* Dominio: ""
* Mi nombre de usuario (Login): ""
* Servidor de Base de datos que contiene los datos para el reporte: XXX.XXX.XXX.XXX
* Nombre de la base de datos fuera de dominio: BD_ORIGEN_DE_DATOS
* Usuario SQL que se conecta a la base de datos para hacer selects de vistas y obtener los datos del reporte: qsense_reader
* Nombre del reporte: TableroSiepPBIDesktopRS.pbix

## ✳️ Lo que funciona:
* El servicio del motor de base de datos se ejecuta correctamente
* Las bases de datos ReportServer y ReportServerTempDB se crearon correctamente en SERVERPBIRS
* Al guardar el reporte para ser publicado en Report Server no dio errores

## 🛠️ Lo configurado:
* Power Bi Desktop RS
   * Orígenes de datos: Los datos se obtienen de 3 vistas que se encuentran en la base de datos BD_ORIGEN_DE_DATOS por DirectQuery

* Report Server Configuration Manager
  * Report Server Connection:
    * Nombre del servidor: SERVERPBIRS
    * Instancia: PBIRS
  * Cuenta de servicio
    * Usar cuenta integrada: Cuenta de servicio Virtual
  * Dirección URL del servicio web
    * Directorio Virtual: ReportServer
    * Dirección IP: Todas Asignadas
    * Puerto TCP: 80
    * Certificado HTTPS: (No seleccionado)
    * Direcciones URL: http://SERVERPBIRS/ReportServer
  * Base de datos
    * Nombre de SQL Server: SERVERPBIRS
    * Nombre de la base de datos: ReportServer
    * Modo del servidor de Informes: Nativo
    * Credencial: Cuenta de servicio
    * Inicio de Sesión: NT Service\PowerBiReporServer
    * Contraseña: ************* (No la puedo ver)
  * Dirección URL del Portal Web
    * Directorio Virtual: Reports
    * Direcciones URL: http://SERVERPBIRS/Reports

* En http://SERVERPBIRS/Reports
  * En Origen de Datos del reporte TableroSiepPBIDesktopRS
    * Tipo: SQL
    * Cadena de conexión: XXX.XXX.XXX.XXX; BD_ORIGEN_DE_DATOS
    * Credenciales:
      * Tipo de autenticación: Autenticación básica
      * Nombre de usuario: qsense_reader
      Al probar conexión funciona correctamente

## ❌ El Problema
* Al ingresar desde la web http://SERVERPBIRS/Reports se carga la pagina pero al intentar visualizar el reporte da el error da el siguiente error: "…No se pudo establecer una conexión con el servidor de Analysis Services. Asegúrese de que la cadena de conexión que ha escrito es correcta.: Identificador de la solicitud: a624761b-f782-2415-c615-7310e88b0aa8 Hora: Mon May 05 2025 07:49:09 GMT-0300 (hora estándar de Argentina) Versión del servicio: /powerbi/libs "

## 📃 Lo que necesito
* Que el reporte se pueda visualizar correctamente en http://SERVERPBIRS/Reports
* Que otros usuarios de dominio además del mio (DOMINIO\Usuario) puedan visualizar el reporte

## ✅ Solución Aplicada
Se deben realizar estos pasos:
  1) En el equipo donde está instalado el Report Server hay que crear la variable de entorno PBI_SQL_TRUSTED_SERVERS y agregar los servidores de confianza a los que puede acceder. En este caso es al servidor donde está la base de datos de la cual se obtienen la info del reporte
https://community.fabric.microsoft.com/t5/Report-Server/Power-BI-report-Server-issue-with-Direct-Query-to-SQL-Server/m-p/4009188

  ![Captura01](./img/Captura01.png)

  2) En la opción de “Configuración del sitio” --> “Seguridad” hay que agregar los usuarios NT que van a acceder al sitio web de Report Server donde se visualizan los reportes

  ![Captura02](./img/Captura02.png)

  3) En la opción de Seguridad del Reporte hay que agregar los usuarios NT que van a acceder al reporte específicamente

  ![Captura03](./img/Captura03.png)

  4) En la opción de Orígenes de datos del Reporte se debe ingresar las credenciales que deben utilizarse para que el reporte se conecte a la base de datos

  ![Captura04](./img/Captura04.png)


