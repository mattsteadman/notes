## Joins

Join-predicate - The condition specified after ON in a JOIN statement

#### CROSS JOIN:
Like the Cartesain product of rows from the tables in the join.

- Explicit: 

        SELECT *
        FROM employee CROSS JOIN department;

- Implicit:

        SELECT *
        FROM employee, department;

#### INNER JOIN:
Joins the intersection between the two columns in the joined tables

- Explicit [INNER is optional]:

        SELECT employee.LastName, employee.DepartmentID, department.DepartmentName 
        FROM employee 
        INNER JOIN department ON
        employee.DepartmentID = department.DepartmentID

- Implicit:

        SELECT *
        FROM employee, department
        WHERE employee.DepartmentID = department.DepartmentID;

    - Equi-join:
        A type of inner join where the join-predicate looks like a.c = b.c. 
        In this case, "ON a.c = b.c" can be replaced with "USING (c)".
        An inner join with a "a.c = b.c" join-predicate is called an equi-join 
        whether or not the USING notation is used. USING is not supported by
        MS SQL Server.

        - Example:
        
                SELECT *
                FROM employee INNER JOIN department USING (DepartmentID);

        - Natural join:
            A special case of an equi-join. It is the "natural" way to join
            two tables that include common column names. The natural join of
            two tables R and S is the set of all combinations of rows in 
            R and S that are equal on their common column ("attribute") names.
            - Natural join is the relational counterpart to a logical AND.
            - A natural join doesn't require a join-predicate, because it uses
            common column names as the implicit predicate.
            - Natural joins are considered harmful, because inadvertantly naming
            two unrelated columns the same in two different tables can create 
            unintented effects.

            - The syntax for a natural join is:
            
                    SELECT *
                    FROM employee NATURAL JOIN department;

#### OUTER JOIN:
An outer join will retain rows that don't satisfy the join-predicate, leaving some rows NULL.

A LEFT outer join will retain rows in the table on the LEFT side of the JOIN keyword
A RIGHT outer join will retain rows in the table to the RIGHT side of the JOIN keyword.
A FULL outer join will retain rows in the tables on BOTH sides of the JOIN keyword.

You can also call these a left join, right join, full join.

There is no implicit notation for outer joins.

- LEFT OUTER:

        SELECT *
        FROM employee LEFT OUTER JOIN department 
          ON employee.DepartmentID = department.DepartmentID;

- RIGHT OUTER:

        SELECT *
        FROM employee RIGHT OUTER JOIN department
          ON employee.DepartmentID = department.DepartmentID;

- FULL OUTER:

        SELECT *
        FROM employee FULL OUTER JOIN department
          ON employee.DepartmentID = department.DepartmentID;
