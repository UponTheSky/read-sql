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

### numeric data
- ints: `tinyint`, `smallint`, `mediumint`, `int`, `bigint` / signed vs unsigned
- floats: `float(p, s)`, `double(p, s)` => `p` for precision(allowable total digit number) / `s` for scale(for only the right side)