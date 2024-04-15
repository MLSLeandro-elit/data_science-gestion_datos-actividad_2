# Actividad 2 - Consultas

Presentado por:  
Leandro Muñoz Ossa  
Mónica Antolínez Becerra  
Ricardo Buitrago Umaña  

El Modelo Relacional de la base de datos es el siguiente:  
![imgen][img_1.0]

## A. SQL 1

Listar todos los cursos ofrecidos en la carrera de Ingeniería de Sistemas. Para este caso se debe tener en cuenta que un mismo curso puede ser dictado por profesores diferentes, en salones diferentes y en horarios diferentes. Se deben obtener los siguientes datos: el nombre del curso, el nombre completo del profesor que dicta el curso, el salón en que se dicta el curso y la hora en la que se dicta del curso.

```sql
SELECT 
    c.NOMBRE AS "Nombre del Curso",
    u.NOMBRE || ' ' || u.APELLIDO AS "Nombre del Profesor",
    s.NOMBRE AS "Salón",
    cc.HORA_INICIO || ' - ' || cc.HORA_FIN AS "Horario"
FROM 
    CURSOS c
    INNER JOIN CURSOS_CARRERAS ccarr ON c.ID_CURSO = ccarr.ID_CURSO
    INNER JOIN CARRERAS ca ON ccarr.ID_CARRERA = ca.ID_CARRERA
    INNER JOIN CALENDARIO_CURSOS cc ON c.ID_CURSO = cc.ID_CURSO
    INNER JOIN SALONES s ON cc.ID_SALON = s.ID_SALON
    INNER JOIN USUARIOS u ON cc.ID_PROFESOR = u.ID_USUARIO
WHERE 
    ca.ID_CARRERA = 1
GROUP BY 
    c.NOMBRE, 
    u.NOMBRE, 
    u.APELLIDO, 
    s.NOMBRE, 
    cc.HORA_INICIO, 
    cc.HORA_FIN;
```

### Desglose de la consulta

1. **Selección de columnas**: Selecciona el nombre del curso, el nombre completo del profesor (concatenando nombre y apellido), el nombre del salón donde se dicta y el horario del curso (concatenando hora de inicio y fin).

2. **Joins**:
   - **`CURSOS` y `CURSOS_CARRERAS`**: Para obtener los cursos que pertenecen a una carrera específica.
   - **`CARRERAS`**: Para especificar que la carrera sea Ingeniería de Sistemas.
   - **`CALENDARIO_CURSOS`**: Para obtener los datos del calendario de cada curso.
   - **`SALONES`**: Para saber en qué salón se imparte cada sesión del curso.
   - **`USUARIOS`**: Para obtener la información del profesor que imparte el curso.

3. **Condición**:
   - Se filtra por el id de la carrera (`ca.ID_CARRERA`), para asegurarse de que sean los de la carrera de Ingeniería de Sistemas, que se identifica con el código 1.

4. **Agrupar por**:  
   - La cláusula GROUP BY en la consulta agrupa los cursos de Ingeniería de Sistemas por nombre del curso (`c.NOMBRE`), nombre del profesor (`u.NOMBRE`, `u.APELLIDO`), salón (`s.NOMBRE`) y horario (`cc.HORA_INICIO` - `cc.HORA_FIN`), asegurando que se muestren todas las combinaciones únicas de estos elementos. Esto evita duplicados en el resultado final y organiza la información para cada grupo de datos.
     
### Resultado de la Consulta

La consulta devuelve un conjunto de datos que muestra los cursos ofrecidos en la carrera de Ingeniería de Sistemas.

- **Número de filas**: La consulta arrojó un total de **22** filas como resultado.
- **Contenido de las filas**: Cada fila contiene el nombre del curso junto con detalles como el profesor, el salón y el horario de cada curso.

A continuación se presenta el detalle de las 22 filas obtenidas:

| Nombre del Curso                      | Nombre del Profesor | Salón | Horario           |
|---------------------------------------|---------------------|-------|-------------------|
| Desarrollo de Videouegos              | Ben Stone           | 3.2   | 02:00 PM - 05:00 PM|
| Programación Paralela                 | Ulrich Nielsen      | 3.3   | 08:00 AM - 10:00 AM|
| Introducción a la Seguridad Informática | Robert Vance       | 3.5   | 04:00 PM - 06:00 PM|
| Lógica Digital y Lenguaje de Máquina  | Grace Stone         | 1.2   | 02:00 PM - 04:00 PM|
| Introducción a la Programación        | Ben Stone           | 1.4   | 08:00 AM - 10:00 AM|
| Electricidad y Magnetismo             | Saanvi Bahl         | 1.5   | 04:00 PM - 06:00 PM|
| Cálculo Multivariado                  | Jared Vásquez       | 2.1   | 02:00 PM - 04:00 PM|
| Cinemática y Dinámica                 | Saanvi Bahl         | 2.2   | 02:00 PM - 04:00 PM|
| Optimización Matemática               | Jared Vásquez       | 2.2   | 02:00 PM - 04:00 PM|
| Cálculo Diferencial                   | Grace Stone         | 2.5   | 08:00 AM - 10:00 AM|
| Arquitectura del Computador           | Grace Stone         | 2.6   | 04:00 PM - 06:00 PM|
| Álgebra Lineal                        | Grace Stone         | 3.1   | 10:00 AM - 12:00 PM|
| Cálculo Integral                      | Jared Vásquez       | 3.3   | 10:00 AM - 12:00 PM|
| Aprendizaje Automático                | Robert Vance        | 1.1   | 04:00 PM - 06:00 PM|
| Optimización Matemática               | Ulrich Nielsen      | 1.3   | 08:00 AM - 10:00 AM|
| Práctica Estudiantil                  | Jared Vásquez       | 1.6   | 02:00 PM - 04:00 PM|
| Teología                              | Zeke Landon         | 1.8   | 10:00 AM - 12:00 PM|
| Ética                                 | Mikaela Stone       | 3.1   | 08:00 AM - 10:00 AM|
| Estructuras de Datos                  | Ben Stone           | 3.4   | 10:00 AM - 12:00 PM|
| Técnicas y Prácticas de Programación  | Ben Stone           | 3.4   | 04:00 PM - 06:00 PM|
| Ética                                 | Jared Vásquez       | 3.7   | 08:00 AM - 10:00 AM|
| Sistemas Inteligentes                 | Robert Vance        | 2.2   | 10:00 AM - 12:00 PM|


___

## B. SQL 2

Obtener la lista de profesores que dictan cursos pertenecientes a la facultad de Humanidades. Se deben obtener los siguientes datos: el nombre completo del profesor y el nombre del curso que dicta.

```sql
SELECT 
    u.NOMBRE || ' ' || u.APELLIDO AS "Nombre Completo del Profesor",
    c.NOMBRE AS "Nombre del Curso"
FROM 
    CURSOS c
    INNER JOIN CALENDARIO_CURSOS cc ON c.ID_CURSO = cc.ID_CURSO
    INNER JOIN USUARIOS u ON cc.ID_PROFESOR = u.ID_USUARIO
WHERE 
    c.ID_FACULTAD = 2;
```

### Descripción de la consulta

1. **Selección de columnas**: Se seleccionan dos campos: el nombre completo del profesor (concatenando el nombre y apellido) y el nombre del curso que dicta.

2. **Joins**:
   - **`CURSOS` y `CALENDARIO_CURSOS`**: Se unen para relacionar los cursos con su correspondiente calendario y obtener el detalle de quién los dicta.
   - **`USUARIOS`**: Se une con `CALENDARIO_CURSOS` para obtener la información del profesor asociado a cada sesión de curso.

3. **Condición**: Se filtra por la `ID_FACULTAD` de los cursos para asegurarse de que sean los de la facultad de Humanidades, que se identifica con el código 2.

### Resultado de la consulta

La consulta devuelve un conjunto de datos que muestra la lista de profesores que dictan cursos pertenecientes a la facultad de Humanidades.

- **Número de filas**: La consulta arrojó un total de 6 filas como resultado.
- **Contenido de las filas**: Cada fila contiene el nombre completo del profesor y el nombre del curso que dicta.

A continuación se presenta el detalla de las 6 filas obtenidas:

| Nombre Completo del Profesor | Nombre del Curso           |
|------------------------------|----------------------------|
| Zeke Landon                  | Teología                   |
| Zeke Landon                  | Filosofía Antigua          |
| Zeke Landon                  | Antropología Filosófica    |
| Zeke Landon                  | Filosofía de la Ciencia    |
| Mikaela Stone                | Ética                      |
| Jared Vásquez                | Ética                      |

___

## C. SQL 3

Obtener la lista de profesores que dictan cursos en dos carreras diferentes. Se deben obtener los siguientes datos: el nombre completo del profesor, el nombre del curso que dicta y el nombre de la carrera a la que pertenece el curso. Pista: averigua cómo funciona la sentencia `WITH`.

```sql
WITH profe_dos_carreras as (
    SELECT
        cal.id_profesor 
        from calendario_cursos cal
        join usuarios u on cal.id_profesor = u.id_usuario and u.id_rol=2
        join cursos cu on cu.id_curso = cal.id_curso
        join cursos_carreras cc on cc.id_curso = cu.id_curso
        join carreras c on c.id_carrera = cc.id_carrera
        group by (cal.id_profesor)
        having count (distinct c.nombre)=2
 )   
select distinct
    u2.nombre || ' ' || u2.apellido as profesor, cu2.nombre as curso, c2.nombre as carrera
    from    profe_dos_carreras pc
    join usuarios u2 on u2.id_usuario = pc.id_profesor
    join calendario_cursos cal2 on cal2.id_profesor = pc.id_profesor
    join cursos cu2 on cu2.id_curso = cal2.id_curso
    join cursos_carreras cc2 on cc2.id_curso = cu2.id_curso
    join carreras c2 on c2.id_carrera = cc2.id_carrera
    ORDER BY profesor ASC;
```

### Explicación de la Consulta

1. **Common Table Expression (CTE) `WITH`**:
   - `profe_dos_carreras`: Esta CTE selecciona los profesores que imparten cursos en dos carreras diferentes.
   
2. **Selección de Columnas en la CTE**:
   - `cal.id_profesor`: Identificador del profesor para futuras agrupaciones.
   
3. **Joins**:
   - Los `JOIN` conectan las tablas `calendario_cursos`, `usuarios`, `cursos`, `cursos_carreras`, y `carreras` para obtener una vista completa de la información relacionada con los cursos que imparten los profesores y las carreras asociadas a estos cursos.
   
4. **Agrupación y Filtro**:
   - La agrupación `GROUP BY (cal.id_profesor)` asegura que cada profesor aparezca solo una vez en el conjunto de resultados.
   - El filtro `HAVING COUNT (DISTINCT c.nombre) = 2` asegura que solo se incluyan los profesores que enseñan en exactamente dos carreras diferentes.

5. **Selección Final**:
   - La consulta final selecciona el nombre completo del profesor, el nombre del curso y el nombre de la carrera en la que imparte los cursos, utilizando los datos obtenidos de la CTE `profe_dos_carreras` y los joins con otras tablas.
   
6. **Ordenamiento**:
   - Los resultados se ordenan por el nombre del profesor en orden ascendente para facilitar la legibilidad y la organización de los datos.

### Resultado de la consulta

La consulta devuelve una lista de profesores que dictan cursos en dos carreras diferentes, junto con el nombre del curso y el nombre de la carrera a la que pertenece cada curso.

- **Número de filas**: La consulta arrojó un total de 13 filas como resultado.
- **Contenido de las filas**: Cada fila contiene el nombre completo del profesor, el nombre del curso que dicta y el nombre de la carrera a la que pertenece dicho curso.

A continuación se presenta el detalle de las 13 filas obtenidas:

| Profesor           | Curso                                 | Carrera                    |
|--------------------|---------------------------------------|----------------------------|
| Ben Stone          | Desarrollo de Videouegos              | Ingeniería de Sistemas     |
| Ben Stone          | Estructuras de Datos                  | Ingeniería de Sistemas     |
| Ben Stone          | Estructuras de Datos                  | Matemáticas Aplicadas      |
| Ben Stone          | Introducción a la Programación        | Ingeniería de Sistemas     |
| Ben Stone          | Técnicas y Prácticas de Programación  | Ingeniería de Sistemas     |
| Claudia Tiedemann  | Diseño Mecánico                       | Ingeniería Mecánica        |
| Claudia Tiedemann  | Máquinas Térmicas e Hidráulicas       | Ingeniería Mecánica        |
| Claudia Tiedemann  | Propiedades de los Materiales         | Ingeniería Mecánica        |
| Claudia Tiedemann  | Química y Ciencia de Materiales       | Ingeniería Mecánica        |
| Claudia Tiedemann  | Álgebra Moderna                       | Matemáticas Aplicadas      |
| Ulrich Nielsen     | Optimización Matemática               | Ingeniería de Sistemas     |
| Ulrich Nielsen     | Optimización Matemática               | Matemáticas Aplicadas      |
| Ulrich Nielsen     | Programación Paralela                 | Ingeniería de Sistemas     |

___

## D. SQL 4

Para un estudiante en particular, obtener el listado de cursos que puede matricular. Para este caso particular se debe tener en cuenta que los cursos se dictan en semestres diferentes, pertenecen a carreras diferentes y un estudiante no puede matricular un curso que ya se encuentre matriculado. Se deben obtener los siguientes datos: el nombre del curso, el nombre de la carrera a la que pertenece y el semestre en el que se ubica. Pista: averigua el uso de la sentencia `NOT EXISTS`.

```sql
SELECT 
    c.NOMBRE AS "Nombre del Curso",
    ca.NOMBRE AS "Nombre de la Carrera",
    cc.SEMESTRE AS "Semestre"
FROM 
    CURSOS c
    INNER JOIN CURSOS_CARRERAS cc ON c.ID_CURSO = cc.ID_CURSO
    INNER JOIN CARRERAS ca ON cc.ID_CARRERA = ca.ID_CARRERA
    INNER JOIN USUARIOS u ON u.ID_USUARIO = &IDESTUDIANTE
WHERE 
    ca.ID_CARRERA IN (
        SELECT ce.ID_CARRERA
        FROM CARRERAS_ESTUDIANTES ce
        WHERE ce.ID_USUARIO = &IDESTUDIANTE
    )
    AND cc.SEMESTRE = u.SEMESTRE
    AND NOT EXISTS (
        SELECT 1
        FROM CURSOS_ESTUDIANTES ce
        INNER JOIN CALENDARIO_CURSOS cc2 ON ce.ID_CALENDARIO = cc2.ID_CALENDARIO
        WHERE ce.ID_USUARIO = &IDESTUDIANTE AND cc2.ID_CURSO = c.ID_CURSO
    )
ORDER BY 
    cc.SEMESTRE, ca.NOMBRE, c.NOMBRE;
```

### Explicación de la consulta

1. **Selección de columnas**
   - `c.NOMBRE AS "Nombre del Curso"`: Se selecciona el nombre del curso de la tabla `CURSOS`.
   - `ca.NOMBRE AS "Nombre de la Carrera"`: Se selecciona el nombre de la carrera de la tabla `CARRERAS`.
   - `cc.SEMESTRE AS "Semestre"`: Se selecciona el semestre del curso de la tabla `CURSOS_CARRERAS`.

2. **Joins**
   - `INNER JOIN CURSOS_CARRERAS cc ON c.ID_CURSO = cc.ID_CURSO`: Este join vincula los cursos con las carreras y los semestres a través de la tabla `CURSOS_CARRERAS`.
   - `INNER JOIN CARRERAS ca ON cc.ID_CARRERA = ca.ID_CARRERA`: Este join asegura que cada curso está correctamente asociado con su carrera correspondiente.
   - `INNER JOIN USUARIOS u ON u.ID_USUARIO = &IDESTUDIANTE`: para obtener los detalles del estudiante, especialmente el semestre en el que está actualmente matriculado.

3. **Filtrado de carreras mediante subconsulta**
   - `ca.ID_CARRERA IN (...)`: Esta cláusula se utiliza para asegurar que los cursos mostrados sean solo aquellos que pertenecen a las carreras en las que el estudiante está matriculado. La subconsulta:

     ```sql
     SELECT ce.ID_CARRERA
     FROM CARRERAS_ESTUDIANTES ce
     WHERE ce.ID_USUARIO = &IDESTUDIANTE
     ```

     selecciona las `ID_CARRERA` de la tabla `CARRERAS_ESTUDIANTES` para un `ID_USUARIO` específico, garantizando que solo se consideren las carreras asociadas a dicho estudiante.

4. **NOT EXISTS**
   - La cláusula `NOT EXISTS` se usa para asegurar que el estudiante no esté ya matriculado en un curso. Dentro de esta subconsulta, se realiza un join entre `CURSOS_ESTUDIANTES` y `CALENDARIO_CURSOS` para obtener el `ID_CURSO` relacionado con el calendario del curso. Esto verifica que no exista un registro del estudiante con ese `ID_CURSO` en `CURSOS_ESTUDIANTES`, efectivamente filtrando los cursos en los que el estudiante ya está inscrito.

5. **Parámetro `&IDESTUDIANTE`**
   - `&IDESTUDIANTE` es un parámetro utilizado en la consulta para identificar específicamente los cursos relacionados con un estudiante dado. Se utiliza tanto en la subconsulta de `CARRERAS_ESTUDIANTES` para filtrar las carreras en las que el estudiante está matriculado como en la cláusula `NOT EXISTS` para asegurar que no se incluyan cursos en los que el estudiante ya está inscrito.

6. **Ordenamiento**
   - La consulta ordena los resultados por semestre, nombre de la carrera y nombre del curso usando `ORDER BY cc.SEMESTRE, ca.NOMBRE, c.NOMBRE`. Esto ayuda a presentar la información de manera organizada, facilitando al estudiante la visualización de sus opciones de matriculación en un formato lógico y estructurado.

### Resultado de la Consulta

La consulta devuelve una lista de cursos disponibles para el estudiante con ID = 17 en el quinto semestre de Ingeniería de Sistemas, asegurándose de que no esté ya matriculado en ellos. Los cursos listados incluyen asignaturas esenciales de su carrera para este periodo académico.

- **Número de filas**: La consulta arrojó un total de 4 filas como resultado.
- **Contenido de las filas**: Cada fila contiene el nombre del curso, el nombre de la carrera y el semestre.

A continuación se presenta el detalle de las 4 filas obtenidas.

| Nombre del Curso             | Nombre de la Carrera   | Semestre |
|------------------------------|------------------------|----------|
| Computabilidad y Complejidad | Ingeniería de Sistemas | 5        |
| Computación Gráfica          | Ingeniería de Sistemas | 5        |
| Comunicación de Datos        | Ingeniería de Sistemas | 5        |
| Electricidad y Magnetismo    | Ingeniería de Sistemas | 5        |

___

## E. SQL 5

Listar los estudiantes que se han inscrito a un curso determinado. Se deben obtener los siguientes datos: el nombre completo del estudiante y el nombre del curso.

```sql
SELECT 
    u.NOMBRE || ' ' || u.APELLIDO AS "Nombre del Estudiante", 
    c.NOMBRE AS "Nombre del Curso"
FROM 
    CURSOS_ESTUDIANTES ce
    INNER JOIN USUARIOS u ON ce.ID_USUARIO = u.ID_USUARIO
    INNER JOIN CALENDARIO_CURSOS cc ON ce.ID_CALENDARIO = cc.ID_CALENDARIO
    INNER JOIN CURSOS c ON cc.ID_CURSO = c.ID_CURSO
ORDER BY
    2, 1;
```

### Detalle de la consulta

1. **Selección de columnas**:
   - `u.NOMBRE || ' ' || u.APELLIDO`: Concatena el nombre y el apellido del estudiante para obtener el nombre completo. Se utiliza el operador `||` para concatenar los valores y se le asigna el alias "Nombre del Estudiante".
   - `c.NOMBRE`: Selecciona el nombre del curso y se le asigna el alias "Nombre del Curso".

2. **Joins**:
   - FROM CURSOS_ESTUDIANTES ce`: Se especifica la tabla principal de donde se obtendrán los datos de los estudiantes inscritos en cursos.
   - `JOIN USUARIOS u ON ce.ID_USUARIO = u.ID_USUARIO`: Se realiza un join con la tabla de usuarios para obtener los datos del estudiante. Se relaciona utilizando el ID de usuario.
   - `JOIN CALENDARIO_CURSOS cc ON ce.ID_CALENDARIO = cc.ID_CALENDARIO`: Se une con la tabla de calendario de cursos para obtener los cursos en los que están inscritos los estudiantes. Se relaciona mediante el ID del calendario de cursos.
   - `JOIN CURSOS c ON cc.ID_CURSO = c.ID_CURSO`: Se une con la tabla de cursos para obtener los nombres de los cursos. Se relaciona mediante el ID del curso.

3. **Ordenamiento**:
   - `ORDER BY 2, 1`: Ordena primero por la segunda columna ("Nombre del Curso") y luego por la primera columna ("Nombre del Estudiante"). El número representa el índice de la columna en el resultado de la consulta.

### Resultado de la consulta

La consulta devuelve una lista de estudiantes que se han inscrito a un curso determinado, mostrando el nombre completo del estudiante y el nombre del curso al que se han inscrito.

- **Número de filas**: La consulta ha devuelto un total de 151 filas como resultado.
- **Contenido de las filas**: Cada fila contiene el nombre completo del estudiante y el nombre del curso al que está inscrito.

A continuación se presenta una muestra de las 10 primeras filas obtenidas:

| Nombre del Estudiante | Nombre del Curso            |
|-----------------------|-----------------------------|
| Franziska Doppler     | Arquitectura del Computador |
| Franziska Doppler     | Arquitectura del Computador |
| Bartosz Tiedemann     | Cinemática y Dinámica       |
| Bartosz Tiedemann     | Cinemática y Dinámica       |
| Bartosz Tiedemann     | Cinemática y Dinámica       |
| Franziska Doppler     | Cinemática y Dinámica       |
| Franziska Doppler     | Cinemática y Dinámica       |
| Franziska Doppler     | Cinemática y Dinámica       |
| Jonas Kahnwald        | Cinemática y Dinámica       |
| Jonas Kahnwald        | Cinemática y Dinámica       |

<!-- Images  -->

[img_1.0]: ./mbd.png "Figure 1.0: Modelo Relacional de la Base de Datos"
