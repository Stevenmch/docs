```markdown
### 📄 Descripción general del proyecto
- **Nombre del código:** NTB_Gen_Write_CSV_to_Datalake
- **Versión:** N/A
- **Explicación general:** Este notebook de Synapse Analytics en PySpark ingiere datos desde archivos CSV o JSON en un datalake, con opciones para transformación, encriptación y optimización.
- **Qué problema resuelve el código:** Automatiza la ingesta de datos desde diferentes fuentes de archivos hacia un datalake, manejando la creación de tablas, el particionamiento, la encriptación de datos sensibles y la optimización del almacenamiento.

### ⚙️ Visión general del sistema
- **Arquitectura del sistema:**
```mermaid
graph LR
A[Archivo CSV/JSON] --> B(Spark Dataframe)
B --> C{Transformación/Encriptación}
C --> D[Datalake (Parquet)]
D --> E((Tabla Externa/Interna))
```
- **Tecnologías utilizadas:**
  - PySpark
  - Azure Synapse Analytics
  - Azure Datalake Storage (ADLS)
  - `mssparkutils`
  - `cryptography`
- **Dependencias:**
  - `pyspark`
  - `ast`
  - `json`
  - `datetime`
  - `math`
  - `re`
  - `uuid`
  - `cryptography`
- **Requisitos del sistema:**
  - Azure Synapse Analytics Workspace
  - Azure Datalake Storage Account
  - Spark Pool configurado en Synapse
- **Prerrequisitos:**
  - Permisos de acceso al Azure Datalake Storage.
  - Configuración de un Spark Pool en Azure Synapse Analytics.
  - Existencia de un Key Vault con la clave de encriptación (si se usa la encriptación).
  - Configuración de linked services para el Key Vault.

### 📦 Guía de uso
- **Cómo usarlo:** El notebook se ejecuta en un entorno de Synapse Analytics Spark, configurando los parámetros de ingesta a través de la variable `notebook_params`. Este proceso carga los datos desde el origen, los transforma según la configuración, y los escribe en el datalake en formato Parquet.
- **Explicación de los pasos (entrada, salida, parámetros):**
  1.  **Configurar los parámetros:** Define la variable `notebook_params` como un string en formato JSON que contiene la configuración de la ingesta. Por ejemplo: `notebook_params = "{'notebook_parameters':{'table_params':{'database_name': 'hub_datalake', 'table_name': 'vendors'},'params':{'account_name': 'azusst1voo929', 'container_name': 'temp', 'file_type': 'json', 'file_path': '/esp-datasets/brs-esp-navision/vendors', 'process_date': '20230412'},'read_args':{'sep': ',', 'header': True, 'inferSchema': True, 'multiline': True}}}"`.
  2.  **Ejecutar las celdas de código:** Ejecuta secuencialmente las celdas del notebook. La celda de parámetros parsea el string JSON y configura las variables necesarias.
  3.  **Cargar los datos:** El código lee los archivos desde la ruta especificada en `file_path` del contenedor ADLS, utilizando las opciones definidas en `read_args`.
  4.  **Transformar y escribir los datos:** Los datos se transforman (ej: encriptación) y se escriben en el datalake en formato Parquet, particionados por `processdate` y `businessdate`. Se crea una tabla externa o interna en el metastore de Synapse.
  5.  **Verificar la ingesta:** Consulta la tabla creada en el datalake para verificar que los datos se hayan ingerido correctamente.
- **Caso de uso de ejemplo:**
```python
from pyspark.sql.functions import lit
# Asumiendo que 'data' es un DataFrame existente
process_date = "20240120"
data = data.withColumn("processdate", lit(process_date))

# Escribir el DataFrame en formato Parquet, particionado por processdate
output_path = "abfss://container@account.dfs.core.windows.net/path/to/output"
data.write.mode("overwrite").partitionBy("processdate").parquet(output_path)
```

### 🔐 Documentación de la API
- **Endpoints:** N/A
- **Formatos de solicitud y respuesta:** N/A
- **Autenticación y autorización:** El acceso a Azure Datalake Storage se gestiona mediante la identidad del workspace de Synapse, que debe tener los roles adecuados (ej: Storage Blob Data Contributor) en la cuenta de almacenamiento.

### 📚 Referencias
- [Azure Synapse Analytics documentation](https://learn.microsoft.com/en-us/azure/synapse-analytics/)
- [PySpark documentation](https://spark.apache.org/docs/latest/api/python/)
- [Azure Datalake Storage documentation](https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction)
```
