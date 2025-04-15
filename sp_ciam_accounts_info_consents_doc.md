
# Documentación del Procedimiento Almacenado: `chl_dwh.sp_ciam_accounts_info_consents`

---

## 📌 Descripción General

Este procedimiento almacena y actualiza información relacionada con los consentimientos de usuarios en la base de datos `chl_dwh`. Realiza las siguientes acciones:

1. Crea la tabla destino si no existe.
2. Deduplica los datos entrantes basándose en el `UID` y la última fecha de actualización.
3. Realiza un `MERGE` (upsert) para insertar o actualizar registros.
4. Elimina las tablas temporales de staging utilizadas durante el proceso.

---

## 🧾 Firma del Procedimiento

```sql
CREATE PROC [chl_dwh].[sp_ciam_accounts_info_consents] @PARAMS NVARCHAR(MAX)
```

> **Nota:** El parámetro `@PARAMS` está definido pero **no es utilizado** dentro del cuerpo del procedimiento.

---

## ⚙️ Lógica del Procedimiento

### 1. Creación de la tabla destino `[chl_dwh].[ciam_accounts_info_consents]`

```sql
IF OBJECT_ID(N'[chl_dwh].[ciam_accounts_info_consents]') IS NULL
```

- Se verifica si la tabla existe.
- Si no existe, se crea con columnas para `UID`, consentimiento para distintos servicios, fechas de actualización, y metainformación.
- Utiliza:
  - **Distribución por HASH en `UID`**
  - **CLUSTERED COLUMNSTORE INDEX** (óptimo para cargas analíticas)

---

### 2. Preparación y deduplicación de datos en staging

```sql
IF OBJECT_ID(N'[chl_stg].[ciam_accounts_info_consents_dedup]') IS NOT NULL DROP TABLE ...
CREATE TABLE ... AS SELECT ...
```

- Se elimina la tabla temporal deduplicada si existe.
- Se crea con los datos más recientes por usuario, usando `ROW_NUMBER()` y `PARTITION BY UID`.

---

### 3. MERGE (Upsert) con la tabla destino

```sql
MERGE [chl_dwh].[ciam_accounts_info_consents] tgt
USING (SELECT * FROM [chl_stg].[ciam_accounts_info_consents_dedup]) src
```

- **Cuando no hay coincidencia (`NOT MATCHED`)**, se inserta un nuevo registro.
- **Cuando hay coincidencia y la nueva fecha (`ciam_lastupdated`) es más reciente**, se actualiza el registro.

---

### 4. Limpieza de tablas temporales

```sql
DROP TABLE [chl_stg].[ciam_accounts_info_consents_dedup]
DROP TABLE [chl_stg].[ciam_accounts_info_consents]
```

- Se eliminan tablas temporales para mantener el entorno limpio.

---

## 📂 Estructura de la Tabla `[chl_dwh].[ciam_accounts_info_consents]`

| Columna                                             | Tipo           | Descripción                                      |
|------------------------------------------------------|----------------|--------------------------------------------------|
| `UID`                                                | NVARCHAR(128)  | Identificador único del usuario                  |
| `cl_newsletters_isConsentGranted`                   | BIT            | Consentimiento para newsletters                  |
| `cl_newsletters_lastConsentModified`                | DATETIME2      | Última fecha de consentimiento para newsletters |
| `privacy_SodexoBRSMovilpassSPA_isConsentGranted`    | BIT            | Consentimiento de privacidad SPA                |
| `privacy_SodexoBRSMovilpassSPA_lastConsentModified` | DATETIME2      | Última fecha de consentimiento privacidad       |
| `terms_SodexoBRSMovilpassSPA_isConsentGranted`      | BIT            | Consentimiento de términos SPA                  |
| `terms_SodexoBRSMovilpassSPA_lastConsentModified`   | DATETIME2      | Última fecha de consentimiento de términos      |
| `ciam_lastupdated`                                  | DATETIME2      | Fecha de última actualización del origen CIAM   |
| `dwhcreated`                                        | DATETIME2      | Fecha de inserción en el DWH                    |
| `dwhlastupdated`                                    | DATETIME2      | Fecha de última modificación en el DWH          |
| `stgprocessdate`                                    | DATE           | Fecha de procesamiento (staging)                |
| `dtlprocessdate`                                    | NVARCHAR(8)    | Fecha de detalle de procesamiento               |
| `filename`                                          | NVARCHAR(4000) | Archivo fuente de los datos                     |

---

## ✅ Consideraciones

- Usa técnicas de procesamiento por lotes seguras y eficientes.
- Emplea deduplicación basada en la lógica de `ROW_NUMBER()`.
- Se asegura de mantener solo los registros más recientes por usuario.
