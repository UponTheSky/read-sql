# A Little Background

## Introduction to Databases
- **database**: a set of related information
- **database system**: a computerized data storage and retrieval mechanism

## Nonrelational Database Systems
- **hierarchical database system**: tree structures(single-parent)
- **network database system**: records and their links(multi-parent)

## The Relational Model
- database = a set of tables
- *redundant data* is used to link records in different tables
- **primary key**: information that uniquely identifies a row in a table
  - should never be allowed to change once a value has been assigned
- **foreign key**: the primary key of another table => links two different table
  - this should only be included in other tables as a connection between two tables

- **normalization**: the process of refining a database design to ensure that each independent piece of information is
in only one place(except for foreign keys)

## Some Terminology
- entity: a topic of interest, such as user, accounts, items, etc.
- row = record
- table = a set of rows, in memory or on storage
- result set = the result of an SQL query in memory

## What Is SQL?
- a language for manipulating data in a (non)relational database => originally for a rdbs
- it goes well with a rdbs: since the result of a query is a table, and inputs of a query can be tables

## SQL Statement Classes
- **schema** statements: define the data structures stored in the database
- **data** statements: manipulate the data structures previously defined using schema statements
- **transaction** statements

- **data dictionary**: a special set of tables storing database elements created via SQL schema statements(**metadata**)

## SQL: A Nonprocedural Language
- **non-procedural**: defining the desired results, but the process by which the results are generated is left to an external agent
- SQL: database engine(*optimizer*) takes care of running a statement
- DB toolkits/APIs for programming languages, CLI 

## SQL Examples
- `SELECT`, `FROM`, `WHERE`
- `INSERT`, `UPDATE`
- `DELETE`

## What Is MySQL?

## SQL Unplugged
- distributed, scalable systems, typically deployed on clusters of commodity servers
- database agnostic, unplugged SQL

## What’s in Store
