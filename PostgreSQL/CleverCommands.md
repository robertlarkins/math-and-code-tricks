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

## Select

### Select particular items from joining table
If there are three tables, _Student_, _Course_, and _Enrollment_, where Enrollment is a joining table between Student and Course.
There are several approaches for selecting all the Students that are not enrolled in a particular course.

The enrollment joining table has the columns: `id`, `student_id`, and `course_id`.
The goal is to find students that are not in the course with id = 3. 

__`Not In`__

```sql
select distinct s.id
from student s
where s.id
    not in (
        select e.student_id
        from enrollment e
        where e.course_id = 3
    )
```
It is not recommended to use `not in`
 - https://wiki.postgresql.org/wiki/Don%27t_Do_This#Don.27t_use_NOT_IN

__`Not Exists`__

```sql
select distinct s.id
from student s
where
    not exists (
        select -- select can be left empty
        from enrollment e
        where e.student_id = s.id
        and e.course_id = 3
    )
```

This post has some more examples
- https://stackoverflow.com/questions/19363481/select-rows-which-are-not-present-in-other-table


## Delete

### Delete duplicate rows

If you want to delete duplicate items from a table based on some combination of value, this can be done using several approaches.
One of these approaches is to use a subquery to find the duplicate rows and number them.

If there is a table `basket` with columns `id` and `fruit`, there could be multiple rows with the values _apple_ and _orange_.

Eg:

id | fruit
---|---
1 | Apple
2 | Banana
3 | Apple
4 | Orange
5 | Apple
6 | Orange

The following query
```sql
SELECT
    id,
    ROW_NUMBER() OVER( PARTITION BY fruit ORDER BY id) AS row_num,
    fruit
FROM
    basket
```
will give the following

id | row_num | fruit
---|---|---
1 | 1 | Apple
3 | 2 | Apple
5 | 3 | Apple
2 | 1 | Banana
4 | 1 | Orange
6 | 2 | Orange

The segment `OVER( PARTITION BY fruit ORDER BY id)` is a window function that partitions the fruit table into subsets based on the fruit column.
Applying the `ROW_NUMBER()` function assigns an integer of one to the first row in the subset, and increases it by one for each subsequent row in the same subset (or called partition).

So then all the row ids where row_num greater than 1 can then be selected:

```sql
SELECT id
FROM
    (SELECT
        id,
        ROW_NUMBER() OVER( PARTITION BY fruit ORDER BY id) AS row_num,
        fruit
    FROM
        basket) t
WHERE t.row_num > 1
```
This will return the ids: `3, 5, 6`.

Combining this with a delete gives
```sql
DELETE FROM basket
WHERE id IN
    (SELECT id
    FROM
        (SELECT
            id,
            ROW_NUMBER() OVER( PARTITION BY fruit ORDER BY id) AS row_num,
            fruit
        FROM
            basket) t
    WHERE t.row_num > 1);
```
meaning the three rows with ids `3, 5, 6` will be deleted from the table.

See:
 - https://www.postgresqltutorial.com/postgresql-row_number/
 - https://www.postgresqltutorial.com/how-to-delete-duplicate-rows-in-postgresql/
 - https://www.postgresql.org/docs/9.1/tutorial-window.html
