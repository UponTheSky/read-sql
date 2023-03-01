# 1
```sql
select actor_id, first_name, last_name
    from actor
        order by last_name, first_name;
```

# 2
```sql
select actor_id, first_name, last_name
    from actor
        where last_name in ('WILLIAMS', 'DAVIS');
```

# 3
```sql
select distinct customer_id 
    from rental
        where date(rental_date) = '2005-07-05';
```

# 4
```sql
select c.email, r.return_date
    from customer as c
        inner join rental as r on c.customer_id = r.customer_id
            where date(r.rental_date) = '2005-06-14'
                order by r.return_date desc;
```
