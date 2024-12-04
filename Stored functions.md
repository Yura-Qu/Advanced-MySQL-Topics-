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

**Example:  find the number of years the employee has been in the company**

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
