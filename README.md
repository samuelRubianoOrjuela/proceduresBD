### Consultas resumen
    Crea procedimientos para cada una de las siguientes consultas.
    ---
1. Calcula la suma del presupuesto de todos los departamentos.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS suma_presupuesto_departamentos;
    CREATE PROCEDURE suma_presupuesto_departamentos()
    BEGIN
    	SELECT SUM(presupuesto) AS presupuesto_total 
    	FROM departamento;
    END $$
    DELIMITER ; $$
    CALL suma_presupuesto_departamentos();
    
    +-------------------+
    | presupuesto_total |
    +-------------------+
    |           1035000 |
    +-------------------+
    ```
    ---
2. Calcula la media del presupuesto de todos los departamentos.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS media_presupuesto_departamentos;
    CREATE PROCEDURE media_presupuesto_departamentos()
    BEGIN
    	SELECT AVG(presupuesto) AS media_presupuesto 
    	FROM departamento;
    END $$
    DELIMITER ; $$
    CALL media_presupuesto_departamentos();

    +--------------------+
    | media_presupuesto  |
    +--------------------+
    | 147857.14285714287 |
    +--------------------+
    ```
    ---
3. Calcula el valor mínimo del presupuesto de todos los departamentos.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS minimo_presupuesto_departamentos;
    CREATE PROCEDURE minimo_presupuesto_departamentos()
    BEGIN
    	SELECT MIN(presupuesto) AS presupuesto_minimo
	FROM departamento;
    END $$
    DELIMITER ; $$
    CALL minimo_presupuesto_departamentos();

    +--------------------+
    | presupuesto_minimo |
    +--------------------+
    |                  0 |
    +--------------------+
    ```
    ---
4. Calcula el nombre del departamento y el presupuesto que tiene asignado, del departamento con menor presupuesto.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS departamento_minimo_presupuesto;
    CREATE PROCEDURE departamento_minimo_presupuesto()
    BEGIN
    	SELECT nombre, presupuesto
	FROM departamento
	ORDER BY presupuesto
	LIMIT 1;
    END $$
    DELIMITER ; $$
    CALL departamento_minimo_presupuesto();
    
    +-----------+-------------+
    | nombre    | presupuesto |
    +-----------+-------------+
    | Proyectos |           0 |
    +-----------+-------------+
    ```
    ---
5. Calcula el valor máximo del presupuesto de todos los departamentos.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS maximo_presupuesto_departamentos;
    CREATE PROCEDURE maximo_presupuesto_departamentos()
    BEGIN
	SELECT MAX(presupuesto) AS presupuesto_maximo
	FROM departamento;
    END $$
    DELIMITER ; $$
    CALL maximo_presupuesto_departamentos();
    
    +--------------------+
    | presupuesto_maximo |
    +--------------------+
    |             375000 |
    +--------------------+
    ```
    ---
6. Calcula el nombre del departamento y el presupuesto que tiene asignado, del departamento con mayor presupuesto.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS departamento_maximo_presupuesto;
    CREATE PROCEDURE departamento_maximo_presupuesto()
    BEGIN
	SELECT nombre, presupuesto
	FROM departamento
	ORDER BY presupuesto DESC
	LIMIT 1;
    END $$
    DELIMITER ; $$
    CALL departamento_maximo_presupuesto();
    
    +--------+-------------+
    | nombre | presupuesto |
    +--------+-------------+
    | I+D    |      375000 |
    +--------+-------------+

    ```
    ---
7. Calcula el número total de empleados que hay en la tabla empleado.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS numero_total_empleados;
    CREATE PROCEDURE numero_total_empleados()
    BEGIN
	SELECT COUNT(id) AS total_empleados
	FROM empleado;
    END $$
    DELIMITER ; $$
    CALL numero_total_empleados();
    
    +-----------------+
    | total_empleados |
    +-----------------+
    |              13 |
    +-----------------+

    ```
    ---
8. Calcula el número de empleados que no tienen NULL en su segundo apellido.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS empleados_con_mama;
    CREATE PROCEDURE empleados_con_mama()
    BEGIN
	SELECT COUNT(id) AS total_empleados
	FROM empleado
	WHERE apellido2 IS NOT NULL;
    END $$
    DELIMITER ; $$
    CALL empleados_con_mama();
    
    +-----------------+
    | total_empleados |
    +-----------------+
    |              11 |
    +-----------------+
    ```
    ---
9. Calcula el número de empleados que hay en cada departamento. Tienes que devolver dos columnas, una con el nombre del departamento y otra con el número de empleados que tiene asignados.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS numero_empleados_departamento;
    CREATE PROCEDURE numero_empleados_departamento()
    BEGIN
	SELECT d.nombre AS nombre_departamento, COUNT(e.id) AS numero_empleados
	FROM departamento d
	LEFT JOIN empleado e ON d.id = e.id_departamento
	GROUP BY d.nombre;
    END $$
    DELIMITER ; $$
    CALL numero_empleados_departamento();

    +---------------------+------------------+
    | nombre_departamento | numero_empleados |
    +---------------------+------------------+
    | Desarrollo          |                3 |
    | Sistemas            |                3 |
    | Recursos Humanos    |                2 |
    | Contabilidad        |                1 |
    | I+D                 |                2 |
    | Proyectos           |                0 |
    | Publicidad          |                0 |
    +---------------------+------------------+
    ```
    ---
10. Calcula el nombre de los departamentos que tienen más de 2 empleados. El resultado debe tener dos columnas, una con el nombre del departamento y otra con el número de empleados que tiene asignados.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS departamentos_2_mas_empleados;
    CREATE PROCEDURE departamentos_2_mas_empleados()
    BEGIN
	SELECT d.nombre AS nombre_departamento, COUNT(e.id) AS numero_empleados
        FROM departamento d
        LEFT JOIN empleado e ON d.id = e.id_departamento
        GROUP BY d.nombre
        HAVING COUNT(e.id) > 2;
    END $$
    DELIMITER ; $$
    CALL departamentos_2_mas_empleados();
    
    +---------------------+------------------+
    | nombre_departamento | numero_empleados |
    +---------------------+------------------+
    | Desarrollo          |                3 |
    | Sistemas            |                3 |
    +---------------------+------------------+
    ```
    ---
11. Calcula el número de empleados que trabajan en cada uno de los departamentos. El resultado de esta consulta también tiene que incluir aquellos departamentos que no tienen ningún empleado asociado.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS numero_empleados_departamento;
    CREATE PROCEDURE numero_empleados_departamento()
    BEGIN
	    SELECT d.nombre AS nombre_departamento, COUNT(e.id) AS 		numero_empleados
	    FROM departamento d
	    LEFT JOIN empleado e ON d.id = e.id_departamento
	    GROUP BY d.nombre;
    END $$
    DELIMITER ; $$
    CALL numero_empleados_departamento();
    
    +---------------------+------------------+
    | nombre_departamento | numero_empleados |
    +---------------------+------------------+
    | Desarrollo          |                3 |
    | Sistemas            |                3 |
    | Recursos Humanos    |                2 |
    | Contabilidad        |                1 |
    | I+D                 |                2 |
    | Proyectos           |                0 |
    | Publicidad          |                0 |
    +---------------------+------------------+
    ```
    ---
12. Calcula el número de empleados que trabajan en cada unos de los departamentos que tienen un presupuesto mayor a 200000 euros.
    ```sql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS numero_empleados_departamento_2;
    CREATE PROCEDURE numero_empleados_departamento_2()
    BEGIN
		SELECT d.nombre AS nombre_departamento, COUNT(e.id) AS 	numero_empleados
		FROM departamento d
		LEFT JOIN empleado e ON d.id = e.id_departamento
		WHERE d.presupuesto > 200000
		GROUP BY d.nombre;
    END $$
    DELIMITER ; $$
    CALL numero_empleados_departamento_2();
    
    +---------------------+------------------+
    | nombre_departamento | numero_empleados |
    +---------------------+------------------+
    | Recursos Humanos    |                2 |
    | I+D                 |                2 |
    +---------------------+------------------+
    ```
    ---
