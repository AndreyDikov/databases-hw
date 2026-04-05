# Условие задачи

Найти информацию о производителях и моделях различных типов транспортных средств (автомобили, мотоциклы и велосипеды), 
которые соответствуют заданным критериям

1. **Автомобили:**

   Извлечь данные о всех автомобилях, которые имеют:

   * Мощность двигателя более **150** лошадиных сил
   * Объем двигателя менее **3** литров
   * Цену менее **35 тысяч** долларов
   
   В выводе должны быть указаны производитель (`maker`), номер модели (`model`), мощность (`horsepower`), объем 
   двигателя (`engine_capacity`) и тип транспортного средства, который будет обозначен как `Car` 

2. **Мотоциклы:**

   Извлечь данные о всех мотоциклах, которые имеют:

   * Мощность двигателя более **150** лошадиных сил
   * Объем двигателя менее **1,5** литров
   * Цену менее **20 тысяч** долларов
   
   В выводе должны быть указаны производитель (`maker`), номер модели (`model`), мощность (`horsepower`), объем 
   двигателя (`engine_capacity`) и тип транспортного средства, который будет обозначен как `Motorcycle`

3. **Велосипеды:**

   Извлечь данные обо всех велосипедах, которые имеют:

   * Количество передач больше **18**
   * Цену менее **4 тысяч** долларов
   
   В выводе должны быть указаны производитель (`maker`), номер модели (`model`), а также `NULL` для мощности и объема 
   двигателя, так как эти характеристики не применимы для велосипедов. Тип транспортного средства будет обозначен как 
   `Bicycle`

4. **Сортировка:**

   Результаты должны быть объединены в один набор данных и отсортированы по мощности в порядке убывания. Для 
   велосипедов, у которых нет значения мощности, они будут располагаться внизу списка

---

# Ожидаемый вывод для тестовых данных

| maker  | model  | horsepower | engine_capacity | vehicle_type |
|--------|--------|------------|-----------------|--------------|
| Toyota | Camry  | 203        | 2.50            | Car          |
| Yamaha | YZF-R1 | 200        | 1.00            | Motorcycle   |
| Honda  | Civic  | 158        | 2.00            | Car          |
| Trek   | Domane | NULL       | NULL            | Bicycle      |
| Giant  | Defy   | NULL       | NULL            | Bicycle      |

---

# Решение

```sql
select v.maker
     , c.model
     , c.horsepower
     , c.engine_capacity
     , 'Car' as type
from vehicle v
join car c
    on v.model = c.model
where c.horsepower > 150
    and c.engine_capacity < 3
    and c.price < 35_000

union all

select v.maker
     , m.model
     , m.horsepower
     , m.engine_capacity
     , 'Motorcycle' as type
from vehicle v
join motorcycle m
    on v.model = m.model
where m.horsepower > 150
    and m.engine_capacity < 1.5
    and m.price < 20_000

union all

select v.maker
     , b.model
     , null         as horsepower
     , null         as engine_capacity
     , 'Bicycle'    as type
from vehicle v
join bicycle b
    on v.model = b.model
where b.gear_count > 18
    and b.price < 4_000

order by horsepower desc nulls last;
```
