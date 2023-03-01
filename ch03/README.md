**remark**: describe the schema of a table => `DESC <Table>`

# Query Mechanics
how does a query work?

- first, you have a database server that requries authentication
- after authentication, we have a *database connection* held by the application that requested the connection
    - this connection is released either by the same application or by the server
    - each connection is assigned an identifier(useful for tracking down some error occurred in this connection)

- now the client is ready to execute queries: the server checks the followings before executing queries:
    - permission to execute the statement
    - permission to access the data desired
    - syntax error in the statement

- but the server doesn't actually dive into executing the query: it hands the query over to the *query optimizer*
    - joining table order / indexes / etc.
    - then it picks an *execution plan*: the server uses it to execute the query

- after execution, the *result set* is returned to the calling application(= just another table containing rows and columns)
    - however, if the query fails in execution, an error message will be found

# Query Clauses
- `select`, `from`, `where`, `group by`, `having`, `order by`: at least 2~3 of these should be used

## select
- one of the last clauses to be executed: you need to know all the possible columns int the final result set to be picked
- you can manipulate all the values in a column with literals, mathematical expressions, built-in or user-defined function calls
- built-in functions: `version()`, `user()`, `database()`(no `FROM` required)
- column aliases: `AS`
- removing duplicates: `SELECT DISTINCT` (but requires data to be sorted, so takes time for large datasets)

## from
- this defines the tables used by a query, along with the means of linking the tables together

### tables
- four different types of tables:
    - permanent tables(stored in a database)
    - derived tables(=result dataset by a subquery and held in memory)
    - temporary tables(=volatile data held in memory)
    - virtual tables(=created using the `CREATE VIEW` statement)

#### Derived (subquery-generated) tables
- **subquery**: a query contained within another query
    - surrounded by parentheses, starting with `SELECT`
- this table is visible from all other query clauses in that query so that it can interact with other tables
- the table data is held in memory for the duration of the query, and then is discarded

#### Temporary tables
- **temporary table**: looks just like permanent tables
- but any data inserted into it will disappear at some point(usually at the end of a transaction, or when the database session is closed)

```sql
CREATE TEMPORARY TABLE actors_j (
    actor_id smallint(5),
    first_name varchar(45),
    last_name varchar(45)
);

INSERT INTO actors_j (
    SELECT actor_id, first_name, last_name 
        FROM actor
            WHERE last_name LIKE 'J%'
);
```

#### Views
- **view**: a query that is stored in the data dictionary
    - looks and acts like a table
    - but there is no data associated with a view

- when you issue a query against a view, your query is merged with the view definition to create a final query to be executed

```sql
CREATE VIEW cust_vw AS
    SELECT customer_id, first_name, last_name, active 
        FROM customer;

SELECT first_name, last_name
    FROM cust_vw
        WHERE active = 0;
```

### table links
- more than one table appears in the `FROM` clause, and we need to link the tables(joining)
- `ON`, `JOIN`

```sql
INNER JOIN rental
    ON customer.customer_id = rental.customer_id
```

### defining table aliases
- you can use `AS` just like using aliases for columns

## where
- for filtering rows
- `AND`, `OR`, `NOT`

## group by & having
- `HAVING` acts like `WHERE` for grouped data

## order by
- for sorting the result
- for more than one column, the sorting priorties go to earlier ones first
- `ASC`, `DESC`
