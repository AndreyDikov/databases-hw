# Условие задачи

Определить, какие автомобили из каждого класса имеют наименьшую среднюю позицию в гонках, и вывести информацию о каждом 
таком автомобиле для данного класса, включая его класс, среднюю позицию и количество гонок, в которых он участвовал. 
Также отсортировать результаты по средней позиции

---

# Ожидаемый вывод для тестовых данных

| car_name              | car_class    | average_position | race_count |
|-----------------------|--------------|------------------|------------|
| Ferrari 488           | Convertible  | 1.0000           | 1          |
| Ford Mustang          | SportsCar    | 1.0000           | 1          |
| Toyota RAV4           | SUV          | 2.0000           | 1          |
| Mercedes-Benz S-Class | Luxury Sedan | 2.0000           | 1          |
| BMW 3 Series          | Sedan        | 3.0000           | 1          |
| Chevrolet Camaro      | Coupe        | 4.0000           | 1          |
| Renault Clio          | Hatchback    | 5.0000           | 1          |
| Ford F-150            | Pickup       | 6.0000           | 1          |

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
    group by c.class
           , c.name
),
ranked as (
    select car_name
         , car_class
         , average_position
         , race_count
         , rank() over (
              partition by car_class
              order by average_position
           ) as rnk
    from car_stats
)
select car_name
     , car_class
     , average_position
     , race_count
from ranked
where rnk = 1
order by average_position;
```
