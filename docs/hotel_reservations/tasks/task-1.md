# Условие задачи

Определить, какие клиенты сделали более двух бронирований в разных отелях, и вывести информацию о каждом таком клиенте, 
включая его имя, электронную почту, телефон, общее количество бронирований, а также список отелей, в которых они 
бронировали номера (объединенные в одно поле через запятую). Также подсчитать среднюю длительность их пребывания (в 
днях) по всем бронированиям. Отсортировать результаты по количеству бронирований в порядке убывания

---

# Ожидаемый вывод для тестовых данных

| name       | email                  | phone       | total_bookings | hotel_list                          | avg_stay_days |
|------------|------------------------|-------------|----------------|-------------------------------------|---------------|
| Bob Brown  | bob.brown@example.com  | +2233445566 | 3              | Grand Hotel, Ocean View Resort      | 3             |
| Ethan Hunt | ethan.hunt@example.com | +5566778899 | 3              | Mountain Retreat, Ocean View Resort | 3             |

---

# Решение

```sql
with customer_stats as (
    select c.id_customer
         , c.name
         , c.email
         , c.phone
         , count(*)                                             as total_bookings
         , count(distinct h.id_hotel)                           as different_hotels
         , string_agg(distinct h.name, ', ' order by h.name)    as hotel_list
         , avg(b.check_out_date - b.check_in_date)              as avg_stay_days
    from customer c
    join booking b
        on b.id_customer = c.id_customer
    join room r
        on r.id_room = b.id_room
    join hotel h
        on h.id_hotel = r.id_hotel
    group by c.id_customer
           , c.name
           , c.email
           , c.phone
)
select name
     , email
     , phone
     , total_bookings
     , hotel_list
     , avg_stay_days
from customer_stats
where total_bookings > 2
    and different_hotels > 1
order by total_bookings desc
       , name;
```
