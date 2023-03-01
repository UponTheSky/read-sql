`WHERE` clauses of `SELECT`, `UPDATE`, `DELETE`

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

### The between operator

### String ranges

## Membership Conditions

### Using subqueries

### Using not in

## Matching Conditions

### Using wildcards

### Using regular expressions

# Null: That Four-Letter Word

