# Условие задачи

Найти всех сотрудников, которые занимают роль менеджера и имеют подчиненных (то есть число подчиненных больше **0**). 
Для каждого такого сотрудника вывести следующую информацию:

1. `EmployeeID`: идентификатор сотрудника
2. Имя сотрудника
3. Идентификатор менеджера
4. Название отдела, к которому он принадлежит
5. Название роли, которую он занимает
6. Название проектов, к которым он относится (если есть, конкатенированные в одном столбце)
7. Название задач, назначенных этому сотруднику (если есть, конкатенированные в одном столбце)
8. Общее количество подчиненных у каждого сотрудника (включая их подчиненных)
9. Если у сотрудника нет назначенных проектов или задач, отобразить `NULL`

---

# Ожидаемый вывод для тестовых данных

| EmployeeID | EmployeeName     | ManagerID | DepartmentName | RoleName | ProjectNames | TaskNames                                                                             | TotalSubordinates |
|------------|------------------|-----------|----------------|----------|--------------|---------------------------------------------------------------------------------------|-------------------|
| 4          | Алексей Алексеев | 2         | Отдел продаж   | Менеджер | Проект A     | Задача 14: Создание презентации для клиентов, Задача 1: Подготовка отчета по продажам | 4                 |

---

# Решение

```sql
with recursive subordinate_tree as (
    select e.employeeid as manager_employeeid
         , s.employeeid as subordinate_employeeid
    from employees e
    join employees s
        on s.managerid = e.employeeid
    where e.roleid = (
        select roleid
        from roles
        where rolename = 'Менеджер'
    )

    union all

    select st.manager_employeeid
         , e.employeeid as subordinate_employeeid
    from subordinate_tree st
    join employees e
        on e.managerid = st.subordinate_employeeid
),
subordinate_count as (
    select manager_employeeid   as employeeid
         , count(*)             as totalsubordinates
    from subordinate_tree
    group by manager_employeeid
),
project_agg as (
    select p.departmentid
         , string_agg(p.projectname, ', ' order by p.projectname) as projectnames
    from projects p
    group by p.departmentid
),
task_agg as (
    select t.assignedto                                     as employeeid
         , string_agg(t.taskname, ', ' order by t.taskname) as tasknames
    from tasks t
    group by t.assignedto
)
select e.employeeid
     , e.name as employeename
     , e.managerid
     , d.departmentname
     , r.rolename
     , pa.projectnames
     , ta.tasknames
     , sc.totalsubordinates
from employees e
join roles r
    on r.roleid = e.roleid
join subordinate_count sc
    on sc.employeeid = e.employeeid
left join departments d
    on d.departmentid = e.departmentid
left join project_agg pa
    on pa.departmentid = e.departmentid
left join task_agg ta
    on ta.employeeid = e.employeeid
where r.rolename = 'Менеджер'
    and sc.totalsubordinates > 0
order by e.name;
```
