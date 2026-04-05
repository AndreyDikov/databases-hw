# Условие задачи

Определить автомобиль, который имеет наименьшую среднюю позицию в гонках среди всех автомобилей, и вывести информацию 
об этом автомобиле, включая его класс, среднюю позицию, количество гонок, в которых он участвовал, и страну 
производства класса автомобиля. Если несколько автомобилей имеют одинаковую наименьшую среднюю позицию, выбрать один из 
них по алфавиту (по имени автомобиля).

---

# Ожидаемый вывод для тестовых данных

| car_name    | car_class   | average_position | race_count | car_country |
|-------------|-------------|------------------|------------|-------------|
| Ferrari 488 | Convertible | 1.0000           | 1          | Italy       |

---

# Решение

```sql
select c.name           as car_name
     , c.class          as car_class
     , avg(r.position)  as average_position
     , count(*)         as race_count
     , cl.country       as car_country
from cars c
join results r
    on r.car = c.name
join classes cl
    on cl.class = c.class
group by c.name
       , c.class
       , cl.country
order by avg(r.position)
       , c.name
limit 1;
```
