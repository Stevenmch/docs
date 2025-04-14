1. 📄 Descripción general del proyecto
   - Nombre del código: CREATE PROC [chl_dwh].[sp_incremental_charge]
   - Versión: N/A
   - Explicación general: Este procedimiento almacenado realiza una carga incremental de datos desde un archivo Parquet en Azure Data Lake Storage Gen2 a una tabla en un sistema SQL. El procedimiento gestiona la creación de tablas temporales, la carga de datos, la actualización de la columna `business_date`, y la eliminación de datos duplicados basados en un rango de fechas.
   - Qué problema resuelve el código: Resuelve el problema de cargar datos nuevos o actualizados en una tabla existente, evitando la duplicación y asegurando que la tabla contenga solo los datos más recientes dentro de un período definido.

2. ⚙️ Visión general del sistema
   - Arquitectura del sistema:

   ```mermaid
   graph LR
       A[Parquet Files in ADLS Gen2] --> B(sp_incremental_charge);
       B --> C{Check if STG Table Exists};
       C -- Yes --> D[Drop STG Table];
       C -- No --> E[Create STG Table];
       D --> E;
       E --> F[Copy Data into STG Table];
       F --> G[Add business_date Column];
       G --> H[Update business_date Column];
       H --> I[Delete Last Days in SQL Pool Table];
       I --> J[Drop STG Table];
       J --> K[Insert Data into Principal Table];
       K --> L[Final Principal Table];
   ```

   - Tecnologías utilizadas:
     - SQL
     - Azure Data Lake Storage Gen2 (ADLS Gen2)
     - OPENJSON
     - Parquet
   - Dependencias:
     - sp_execute_sql (asumida)
   - Requisitos del sistema:
     - Acceso a Azure Data Lake Storage Gen2.
     - Permisos para crear, modificar y eliminar tablas en el sistema SQL.
     - Permisos para ejecutar procedimientos almacenados.
   - Prerrequisitos:
     - Configuración de la identidad administrada para acceder a ADLS Gen2.
     - Existencia de la base de datos y esquema correspondientes.

3. 📦 Guía de uso
   - Cómo usarlo: El procedimiento almacenado se ejecuta proporcionando un string JSON como parámetro de entrada (`@PARAMS`). Este JSON contiene los parámetros necesarios para la carga incremental.       
   - Explicación de los pasos:
     1.  El procedimiento recibe un string JSON con los parámetros de configuración.
     2.  Extrae los valores del JSON a variables SQL.
     3.  Verifica si la tabla temporal (STG) existe y, si existe, la elimina.
     4.  Crea una tabla temporal (STG) copiando los datos desde un archivo Parquet en ADLS Gen2.
     5.  Agrega una columna `business_date` a la tabla temporal.
     6.  Actualiza la columna `business_date` con el valor proporcionado en los parámetros o con la fecha actual si no se proporciona.
     7.  Determina el rango de fechas para eliminar datos de la tabla principal.
     8.  Elimina los datos correspondientes al rango de fechas de la tabla principal.
     9.  Elimina la tabla temporal (STG).
     10. Inserta los datos de la tabla temporal en la tabla principal.
   - Caso de uso de ejemplo:

   ```sql
   -- Ejemplo de uso del procedimiento almacenado
   DECLARE @PARAMS VARCHAR(1000);
   SET @PARAMS = '{"country_code": "US", "schema_suffix": "dwh", "account_name": "testaccount", "source_name": "sales", "table_name": "transactions", "fecha": "20240101", "business_date": "fecha_creacion"}';

   EXEC [chl_dwh].[sp_incremental_charge] @PARAMS;
   ```

4. 🔐 Documentación de la API
   - Endpoints: Este procedimiento no expone un endpoint HTTP directamente, sino que se ejecuta dentro del entorno SQL.
   - Formatos de solicitud y respuesta:
     - Solicitud: Un string JSON que contiene los parámetros de configuración.
     - Respuesta: No hay una respuesta formal, pero el procedimiento imprime mensajes de estado y errores a través de la función `PRINT`.
   - Autenticación y autorización: La autenticación y autorización se gestionan a nivel de la base de datos SQL. El usuario que ejecuta el procedimiento debe tener los permisos necesarios para acceder a ADLS Gen2 y modificar las tablas correspondientes.

5. 📚 Referencias
   - [Azure Data Lake Storage Gen2](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction)
   - [OPENJSON (Transact-SQL)](https://docs.microsoft.com/en-us/sql/t-sql/functions/openjson-transact-sql?view=sql-server-ver16)
   - [COPY statement](https://learn.microsoft.com/en-us/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest&preserve-view=true)
   - [Managed Identities for Azure Resources](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview)
