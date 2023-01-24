# 1
```sql
start transaction;

update Account 
set avail_balance = avail_balance - 50, last_activity_date = current_date()
where account_id = 123;

insert into Transaction (
    txn_date, account_id, txn_type_cd, amount
) values (
    current_date(), 123, 'D', 50
);

savepoint after_sending_50;

update Account
set avail_balance = avail_balance + 50, last_activity_date = current_date()
where account_id = 789;

insert into Transaction (
    txn_date, account_id, txn_type_cd, amount
) values (
    current_date(), 789, 'C', 50
);

commit;
```