## Stored function structure 
```
CREATE FUNCTION function_name(func_parameter1, func_parameter2, ..)
          RETURN datatype [characteristics]
          func_body
```
**Parameters used:**

1. **function_name:** The unique identifier for the stored function. Must differ from built-in functions and can include database prefix (*database_name.func_name*).
2. **func_parameter:** Input arguments used in function body. Cannot use **IN, OUT, INOUT**. Declared as *func_parameter type*.
3. **datatype:** The type of value returned.
4. **characteristics:** Must specify at least one: { DETERMINISTIC, NO SQL, or READS SQL DATA }.
5. ***func_body*** is the set of Mysql statements that perform operation. It’s structure is as follows:
    
    ```sql
    BEGIN
    
            Mysql Statements
    
            RETURN expression;
    END
    ```
    
    **The function body must contain one RETURN statement.**

**Example 1:  find the number of years the employee has been in the company**

| **emp_id** | **fname** | **lname** | **start_date** |
| --- | --- | --- | --- |
| 1 | Michael | Smith | 2001-06-22 |
| 2 | Susan | Barker | 2002-09-12 |
| 3 | Robert | Tvler | 2000-02-09 |
| 4 | Susan | Hawthorne | 2002-04-24 |

```sql
DELIMITER //

CREATE FUNCTION no_of_years(date1 date) RETURNS int DETERMINISTIC
BEGIN
 DECLARE date2 DATE;
  Select current_date()into date2;
  RETURN year(date2)-year(date1);
END 

//

DELIMITER ;

Select emp_id, fname, lname, 
no_of_years(start_date) as 'years' from employee;
```

| **emp_id** | **fname** | **lname** | **years** |
| --- | --- | --- | --- |
| 1 | Michael | Smith | 18 |
| 2 | Susan | Barker | 17 |
| 3 | Robert | Tvler | 19 |
| 4 | Susan | Hawthorne | 17 |
1. function name: no_of_years
2. function parameter: start_date
3. datatype: int 
4. **characteristics:** DETERMINISTIC

**Example 2:  The travel database** 

https://www.red-gate.com/simple-talk/databases/mysql/working-with-mysql-stored-functions/ 

Dataset: 
```sql
DROP DATABASE IF EXISTS travel;
CREATE DATABASE travel;
USE travel;
DROP TABLE IF EXISTS manufacturers;
CREATE TABLE manufacturers (
  manufacturer_id INT UNSIGNED NOT NULL AUTO_INCREMENT,
  manufacturer VARCHAR(50) NOT NULL,
  create_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  last_update TIMESTAMP NOT NULL 
    DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (manufacturer_id) ) 
ENGINE=InnoDB AUTO_INCREMENT=1001;
DROP TABLE IF EXISTS airplanes;
CREATE TABLE airplanes (
  plane_id INT UNSIGNED NOT NULL AUTO_INCREMENT,
  plane VARCHAR(50) NOT NULL,
  manufacturer_id INT UNSIGNED NOT NULL,
  engine_type VARCHAR(50) NOT NULL,
  engine_count TINYINT NOT NULL,
  max_weight MEDIUMINT UNSIGNED NOT NULL,
  wingspan DECIMAL(5,2) NOT NULL,
  plane_length DECIMAL(5,2) NOT NULL,
  parking_area INT GENERATED ALWAYS AS 
        ((wingspan * plane_length)) STORED,
  icao_code CHAR(4) NOT NULL,
  create_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  last_update TIMESTAMP NOT NULL 
    DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (plane_id),
  CONSTRAINT fk_manufacturer_id FOREIGN KEY (manufacturer_id) 
    REFERENCES manufacturers (manufacturer_id) ) 
ENGINE=InnoDB AUTO_INCREMENT=101;
INSERT INTO manufacturers (manufacturer)
VALUES ('Airbus'), ('Beechcraft'), ('Piper');
INSERT INTO airplanes 
  (plane, manufacturer_id, engine_type, engine_count, 
    max_weight, wingspan, plane_length, icao_code)
VALUES 
  ('A380-800', 1001, 'jet', 4, 1267658, 261.65, 238.62, 'A388'),
  ('A319neo Sharklet', 1001, 'jet', 2, 166449, 117.45, 111.02, 'A319'),
  ('ACJ320neo (Corporate Jet version)', 1001, 'jet', 2, 174165, 
               117.45, 123.27, 'A320'),
  ('A300-200 (A300-C4-200, F4-200)', 1001, 'jet', 2, 363760, 147.08, 
               175.50, 'A30B'),
  ('Beech 390 Premier I, IA, II (Raytheon Premier I)', 1002, 'jet', 
               2, 12500, 44.50, 46.00, 'PRM1'),
  ('Beechjet 400 (from/same as MU-300-10 Diamond II)', 1002, 'jet', 
              2, 15780, 43.50, 48.42, 'BE40'),
  ('1900D', 1002, 'Turboprop', 2,17120,  57.75, 57.67, 'B190'),
  ('PA-24-400 Comanche', 1003, 'piston', 1, 3600, 36.00, 24.79, 'PA24'),
  ('PA-46-600TP Malibu Meridian, M600', 1003, 'Turboprop', 1, 6000, 
         43.17, 29.60, 'P46T'),
  ('J-3 Cub', 1003, 'piston', 1, 1220, 38.00, 22.42, 'J3');
```


