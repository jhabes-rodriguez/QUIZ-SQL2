SQL Queries 

Este documento contiene las consultas SQL utilizadas para el quiz de SQL2

---

# PUNTO 1:

## 1. Todos los registros de la tabla tic_2023.
```sql
SELECT*
FROM tic_2023;
```

## 2. Únicamente los valores distintos que existen en la columna ACTIVIDAD_NOMBRE de la tabla tic_2023. Sin repetidos
```sql
SELECT DISTINCT ACTIVIDAD_NOMBRE
FROM tic_2023;
```

# PUNTO 2:
## 1. Extraer de la tabla tic_2022 las siguientes columnas, con los alias indicados y en el orden exacto que se especifica:
```sql
SELECT 
    SEDE_CODIGO AS codigo_sede,
    PERIODO_ANIO AS anio_reporte,
    ACTIVIDAD_CODIGO AS cod_actividad,
    ACTIVIDAD_NOMBRE AS nombre_actividad
FROM tic_2022
ORDER BY SEDE_CODIGO ASC
LIMIT 50;
```

# PUNTO 3:
## 1. Tabla tic_sedes_resumen
```sql 
CREATE TABLE tic_sedes_resumen (
    resumen_id INT PRIMARY KEY,
    sede_codigo INT NOT NULL,
    anio INT NOT NULL,
    total_actividades INT,
    tiene_internet BOOLEAN,
    fecha_carga DATETIME,
    
    CONSTRAINT DIFERENTES UNIQUE (sede_codigo, anio)
);
```

# PUNTO 4:
## 1.
```sql
SELECT 
    SUBSTR(SEDE_CODIGO, 1, 2) AS codigo_departamento,
    COUNT(DISTINCT SEDE_CODIGO) AS total_sedes_unicas
FROM 
    tic_2023
WHERE 
    ACTIVIDAD_CODIGO = '05'
GROUP BY 
    SUBSTR(SEDE_CODIGO, 1, 2)
HAVING 
    COUNT(DISTINCT SEDE_CODIGO) > 500
ORDER BY 
    total_sedes_unicas DESC;
```

# PUNTO 5:
## 1.
```sql
SELECT 
    tic_2022.SEDE_CODIGO AS codigo_sede,
    COUNT(tic_2022.SEDE_CODIGO) AS total_act_2022,
    COUNT(tic_2023.SEDE_CODIGO) AS total_act_2023,
    COUNT(tic_2023.SEDE_CODIGO) - COUNT(tic_2022.SEDE_CODIGO) AS diferencia,
    CASE 
        WHEN COUNT(tic_2023.SEDE_CODIGO) - COUNT(tic_2022.SEDE_CODIGO) > 0 THEN 'CRECIÓ'
        WHEN COUNT(tic_2023.SEDE_CODIGO) - COUNT(tic_2022.SEDE_CODIGO) < 0 THEN 'DECRECIÓ'
        ELSE 'SIN CAMBIO'
    END AS tendencia
FROM tic_2022
INNER JOIN tic_2023
    ON tic_2022.SEDE_CODIGO = tic_2023.SEDE_CODIGO
GROUP BY tic_2022.SEDE_CODIGO
HAVING COUNT(tic_2022.SEDE_CODIGO) >= 2
ORDER BY diferencia DESC
LIMIT 30;
```
