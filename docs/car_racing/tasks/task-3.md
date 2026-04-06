# Условие задачи

Определить классы автомобилей, которые имеют наименьшую среднюю позицию в гонках, и вывести информацию о каждом 
автомобиле из этих классов, включая его имя, среднюю позицию, количество гонок, в которых он участвовал, страну 
производства класса автомобиля, а также общее количество гонок, в которых участвовали автомобили этих классов. Если 
несколько классов имеют одинаковую среднюю позицию, выбрать все из них

---

# Ожидаемый вывод для тестовых данных

| car_name     | car_class   | average_position | race_count | car_country | total_races |
|--------------|-------------|------------------|------------|-------------|-------------|
| Ferrari 488  | Convertible | 1.0000           | 1          | Italy       | 1           |
| Ford Mustang | SportsCar   | 1.0000           | 1          | USA         | 1           |


---

# Решение

```sql
with car_stats as (
    select c.name           as car_name
         , c.class          as car_class
         , avg(r.position)  as average_position
         , count(*)         as race_count
    from cars c
    join results r
        on r.car = c.name
    group by c.name
           , c.class
),
class_stats as (
    select c.class          as car_class
         , avg(r.position)  as class_average_position
         , count(*)         as total_races
    from cars c
    join results r
        on r.car = c.name
    group by c.class
),
best_classes as (
    select car_class
         , total_races
    from class_stats
    where class_average_position = (
        select min(class_average_position)
        from class_stats
    )
)
select cs.car_name
     , cs.car_class
     , cs.average_position
     , cs.race_count
     , cl.country as car_country
     , bc.total_races
from car_stats cs
join best_classes bc
    on bc.car_class = cs.car_class
join classes cl
    on cl.class = cs.car_class
order by cs.average_position
       , cs.car_name;
```
