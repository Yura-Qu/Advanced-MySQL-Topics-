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
