# Условие задачи

Необходимо провести анализ клиентов, которые сделали более двух бронирований в разных отелях и потратили более **500** 
долларов на свои бронирования. Для этого:

* Определить клиентов, которые сделали более двух бронирований и забронировали номера в более чем одном отеле. Вывести 
  для каждого такого клиента следующие данные: `ID_customer`, имя, общее количество бронирований, общее количество 
  уникальных отелей, в которых они бронировали номера, и общую сумму, потраченную на бронирования

* Также определить клиентов, которые потратили более **500** долларов на бронирования, и вывести для них `ID_customer`, 
  имя, общую сумму, потраченную на бронирования, и общее количество бронирований

* В результате объединить данные из первых двух пунктов, чтобы получить список клиентов, которые соответствуют условиям 
  обоих запросов. Отобразить поля: `ID_customer`, имя, общее количество бронирований, общую сумму, потраченную на 
  бронирования, и общее количество уникальных отелей

* Результаты отсортировать по общей сумме, потраченной клиентами, в порядке возрастания

---

# Ожидаемый вывод для тестовых данных

| id_customer | name       | total_bookings | total_spent | unique_hotels |
|-------------|------------|----------------|-------------|---------------|
| 4           | Bob Brown  | 3              | 820.00      | 2             |
| 7           | Ethan Hunt | 3              | 850.00      | 2             |

---

# Решение

```sql
with customer_booking_stats as (
    select c.id_customer
         , c.name
         , count(*)                     as total_bookings
         , count(distinct h.id_hotel)   as unique_hotels
         , sum(r.price)                 as total_spent
    from customer c
    join booking b
        on b.id_customer = c.id_customer
    join room r
        on r.id_room = b.id_room
    join hotel h
        on h.id_hotel = r.id_hotel
    group by c.id_customer
           , c.name
)
select id_customer
     , name
     , total_bookings
     , total_spent
     , unique_hotels
from customer_booking_stats
where total_bookings > 2
    and unique_hotels > 1
    and total_spent > 500
order by total_spent;
```
