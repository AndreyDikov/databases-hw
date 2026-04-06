# Условие задачи

Определить, какие автомобили имеют среднюю позицию лучше (меньше) средней позиции всех автомобилей в своем классе (то 
есть автомобилей в классе должно быть минимум два, чтобы выбрать один из них). Вывести информацию об этих автомобилях, 
включая их имя, класс, среднюю позицию, количество гонок, в которых они участвовали, и страну производства класса 
автомобиля. Также отсортировать результаты по классу и затем по средней позиции в порядке возрастания

---

# Ожидаемый вывод для тестовых данных

| car_name     | car_class | average_position | race_count | car_country |
|--------------|-----------|------------------|------------|-------------|
| BMW 3 Series | Sedan     | 3.0              | 1          | Germany     |
| Toyota RAV4  | SUV       | 2.0000           | 1          | Japan       |


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
    select car_class
         , avg(average_position)    as class_average_position
         , count(*)                 as car_count
    from car_stats
    group by car_class
    having count(*) >= 2
)
select cs.car_name
     , cs.car_class
     , cs.average_position
     , cs.race_count
     , cl.country as car_country
from car_stats cs
join class_stats cls
    on cls.car_class = cs.car_class
join classes cl
    on cl.class = cs.car_class
where cs.average_position < cls.class_average_position
order by cs.car_class
       , cs.average_position;
```
