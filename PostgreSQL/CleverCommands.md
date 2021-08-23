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
select distinct e.student_id
from enrollment e
where e.student_id
    not in (
        select e2.student_id
        from enrollment e2
        where e2.course_id = 3
    )
```
It is not recommended to use `not in`
 - https://wiki.postgresql.org/wiki/Don%27t_Do_This#Don.27t_use_NOT_IN

__`Not Exists`__

```sql
select distinct e.student_id
from enrollment e
where
    not exists (
        select -- select can be left empty
        from enrollment e2
        where e.student_id = e2.student_id
        and e2.course_id = 3
    )
```

This post has some more examples
- https://stackoverflow.com/questions/19363481/select-rows-which-are-not-present-in-other-table
