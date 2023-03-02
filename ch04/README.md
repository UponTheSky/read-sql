- `WHERE` clauses of `SELECT`, `UPDATE`, `DELETE`
- `HAVING`: filtering grouped data(ch8)

# Condition Evaluation
- `OR`, `AND`

## Using Parentheses
- using parenthesis

## Using the not Operator
- `NOT`

# Building a Condition
- a *condition* = one or more *expressions* + one or more *operators*

# Condition Types

## Equality Conditions
- `=`

### Inequality conditions
- either `!=` or `<>`

### Data modification using equality conditions
- useful for data modification

```sql
DELETE FROM rental WHERE year(rental_date) = 2004;
```

## Range Conditions
- `<`, `>`, `<=`, `>=`


### The between operator
- `BETWEEN <A> AND <B>`(inclusive, `A` should be lower than `B`)

### String ranges
- **collation**: the order in which the characters within a character set are sorted

## Membership Conditions
- `IN (A, B)`

### Using subqueries
- you can use a subquery to generate a set for you on the fly

### Using not in
- `NOT IN (A, B)`

## Matching Conditions
- how to match a string expression?

### Using wildcards
- `like`
- `_`: exactly one character
- `%`: any number of characters(including 0)

### Using regular expressions
- `WHERE last_name REGEXP '<regex>'`

# Null: That Four-Letter Word
- null is the absence of a value
- examples: not applicable, value not yet known, value undefined

- remark:
  - an expression can be null, but it can never equal to null
  - two nulls are never equal to each other
  - possible pitfall: some values could be null, so be careful when querying such data

- null checking:
  - `IS NULL`(instead of `= NULL`) => it doesn't give any error, so be careful!
  - also, `IS NOT NULL`
