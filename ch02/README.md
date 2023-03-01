# Ch2.

## Donwload and install mysql
- Use Homebrew for easy use of MySQL via CLI: `brew install mysql`
- After finishing install, the root password would not have been setup.
    - restart the server(`mysql.server restart`) and run `mysql_secure_installation` and type in the password for user root
    - thanks to: https://stackoverflow.com/questions/9695362/macosx-homebrew-mysql-root-password

## Using mysql CLI

### start, stop, restart the server
- `mysql.server start/stop/restart`

### Login as the root user
- Run `mysql -u root -p`
- Enter the password you setup in the above setting
- `show databases` to list the current existing dbs in the system

### Adapt a data schema
- in the state of running the server and logined, use `SOURCE <schema_path>`
- add the data to the tables in the db: `SOURCE <data_path>`

### Use the db
- `mysql> USE <db_name>`
- or `mysql -u user -p <db_name>`
- show all the tables: `SHOW FULL TABLES`(or just `SHOW TABLES`)

### quit the server
- type `quit` or `exit`

## mysql data types

### character data
- `char(max_len)`: fixed length => right padded if there are left spaces / 255 bytes
- `varchar(max_len)`: variable length / 65535 bytes

#### character sets
- multi-byte character sets: Japanese, Korean
- `SHOW CHARACTER SET`
- defining the character set for a variable: `varchar(max_len) character set latin1`
- setting the default character set for the entire database: `create database <dbname> character set latin1`

#### text data
- if you want to store data that might exceed the 64Kb limit for `varchar` columns
- `tinytext(255)` / `text(65535)` / `mediumtext`, `longtext` => this varies depending on db servers / no need for `tinytext` and `text` anymore
- exceeding characters will be truncated
- trailing spaces are not removed

#### numeric data
- ints: `tinyint`, `smallint`, `mediumint`, `int`, `bigint` / signed vs unsigned
- floats: `float(p, s)`, `double(p, s)` => `p` for precision(allowable total digit number) / `s` for scale(for only the right side)

#### temporal data
- dates, times: `date`, `datetime`, `timestamp`, `year`, `time`
- string formats: how the data will be constructed and represented

## table creation

### step1: design

### step2: refinement
- **normalization**: the process of ensuring that there are no duplicate or compound columns in your db design

### step3: building sql schema statements
- when you define a table, you need to tell the db server what column(s) will serve as the **primary ke**y for the table
- validation: allowable values for certain columns

```sql
eye_color CHAR(2) CHECK (eye_color IN ('BR', 'BL', 'GR')),

-- or 

eye_color ENUM('BR', 'BL', 'GR')

CONSTRAINT pk_person PRIMARY KEY (person_id)
```

- null: not applicable, unknown yet, empty set(enforcing `not null` could be possible)
- **foreign key constraint**: allowing only the values found in the table specifed(modifiable later via `alter table` statement)

```sql
CONSTRAINT fk_fav_food_person_id FOREIGN KEY (person_id) REFERENCES person (person_id)
```

- checking the schema of a table: `DESCRIBE <your_table_name>`

## populating and modifying tables

### inserting data
- you need to specify: table / columns / values

#### generating nemeric key data
- rather than manually looking up the max id value, 
all db servers on the market provide a robust method for generating numeric keys

- mysql: *auto-increment*

```sql
ALTER TABLE person MODIFY person_id SMALLINT UNSIGNED AUTO_INCREMENT;
```
(setting foreign key available/unavailable: `set foreign_key_checks=0(or 1)`)

#### the insert statement

```sql
INSERT INTO person (
    person_id, fname, lname, eye_color, birth_date
) values (
    null, 'William', 'Turner', 'BR', '1972-05-27'
);
```

### updating data
- can update (possbly multiple) data filtered by `WHERE`
    - so beware that you don't omit `WHERE`, otherwise you'll update every row in the table

```sql
UPDATE person SET 
    street = '1225 Tremont St.',
    city = 'Boston',
    state = 'MA',
    country = 'USA',
    postal_code = '02138',
        WHERE person_id = 1;    
```

### deleting data
- like `UPDATE`, beware of `WHERE` clause

```sql
DELETE FROM person
    WHERE person_id = 2;
```

## when good statements go bad

- non-unique primary key
- non-existent foreign key
- column value violations(in case of `ENUM`)
- invalid data conversions(in case of the datetime formats)

## the sakila database
- droping tables

```sql
DROP TABLE favorite_food;
```