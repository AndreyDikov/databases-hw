# Условие задачи

Найдите производителей (`maker`) и модели всех мотоциклов, которые имеют мощность более **150** лошадиных сил, стоят 
менее **20 тысяч** долларов и являются спортивными (тип `Sport`). Также отсортируйте результаты по мощности в порядке 
убывания

---

# Ожидаемый вывод для тестовых данных

| maker  | model  |
|--------|--------|
| Yamaha | YZF-R1 |

---

# Решение

```sql
select v.maker
     , m.model
from vehicle v
join motorcycle m
    on v.model = m.model
where m.horsepower > 150
    and m.price < 20_000
    and m.type = 'Sport'
order by m.horsepower desc;
```
