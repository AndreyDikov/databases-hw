# Условие задачи

Определить, какие классы автомобилей имеют наибольшее количество автомобилей с низкой средней позицией (больше **3.0**) 
и вывести информацию о каждом автомобиле из этих классов, включая его имя, класс, среднюю позицию, количество гонок, в 
которых он участвовал, страну производства класса автомобиля, а также общее количество гонок для каждого класса. 
Отсортировать результаты по количеству автомобилей с низкой средней позицией

---

# Ожидаемый вывод для тестовых данных

| car_name          | car_class | average_position | race_count | car_country | total_races | low_position_count |
|-------------------|-----------|------------------|------------|-------------|-------------|--------------------|
| Audi A4           | Sedan     | 8.0000           | 1          | Germany     | 2           | 2                  |
| Chevrolet Camaro  | Coupe     | 4.0000           | 1          | USA         | 1           | 1                  |
| Renault Clio      | Hatchback | 5.0000           | 1          | France      | 1           | 1                  |
| Ford F-150        | Pickup    | 6.0000           | 1          | USA         | 1           | 1                  |


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
    select cs.car_class
         , sum(cs.race_count)   as total_races
         , count(*)             as low_position_count
         , count(
               case
                   when cs.average_position > 3.0 then 1
                   end
           )                    as bad_car_count
    from car_stats cs
    group by cs.car_class
),
best_classes as (
    select car_class
         , total_races
         , low_position_count
    from class_stats
    where bad_car_count = (
        select max(bad_car_count)
        from class_stats
    )
)
select cs.car_name
     , cs.car_class
     , cs.average_position
     , cs.race_count
     , cl.country as car_country
     , bc.total_races
     , bc.low_position_count
from car_stats cs
join best_classes bc
    on bc.car_class = cs.car_class
join classes cl
    on cl.class = cs.car_class
where cs.average_position > 3.0
order by bc.low_position_count desc
       , cs.car_name;
```
