# Clever Commands

## Update

### Update using the most recent date

In this situation there are two tables `schedule` and `appointment`, in which schedule can have multiple appointments,
and each appointment has a nullable column `start_date_time`. The following query updates the not-null `latest_start_date_time`
column of the schedule table using the `start_date_time` of the most recent appointment that has a value.
If there are no appointments, or `start_date_time` is null for all of them, then the default value is going to be `1970-1-1`.

```sql
UPDATE schedule
SET latest_start_date_time = 
    COALESCE((
        SELECT MAX(a.start_date_time)
        FROM appointment a
        WHERE a.inspection_id = id),
        '1970-1-1');
```
