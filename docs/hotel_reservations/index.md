# Схема базы данных бронирования отелей

![hotel-reservations-db-diagram](../imgs/hotel-reservations-db-diagram.png)

---

# Скрипты создания таблиц

### Таблица `hotel`
```sql
create table hotel (
    id_hotel    serial          primary key, -- используем serial для автоматической генерации идентификаторов
    name        varchar(255)    not null,
    location    varchar(255)    not null
);
```

### Таблица `room`
```sql
create table room (
    id_room     serial          primary key, -- используем serial для автоматической генерации идентификаторов
    id_hotel    int,
    room_type   varchar(20)     not null check (room_type in ('single', 'double', 'suite')), -- тип номера
    price       decimal(10, 2)  not null,
    capacity    int             not null,
    foreign key (id_hotel) references hotel(id_hotel)
);
```

### Таблица `customer`
```sql
create table customer (
    id_customer serial          primary key, -- используем serial для автоматической генерации идентификаторов
    name        varchar(255)    not null,
    email       varchar(255)    unique not null,
    phone       varchar(20)     not null
);
```

### Таблица `booking`
```sql
create table booking (
    id_booking      serial  primary key, -- используем serial для автоматической генерации идентификаторов
    id_room         int,
    id_customer     int,
    check_in_date   date    not null,
    check_out_date  date    not null,
    foreign key (id_room)       references room(id_room),
    foreign key (id_customer)   references customer(id_customer)
);
```

---

# Скрипты наполнения данными

### Данные для таблицы `hotel`
```sql
insert into hotel (id_hotel, name, location)
values (1, 'grand hotel', 'paris, france'),
       (2, 'ocean view resort', 'miami, usa'),
       (3, 'mountain retreat', 'aspen, usa'),
       (4, 'city center inn', 'new york, usa'),
       (5, 'desert oasis', 'las vegas, usa'),
       (6, 'lakeside lodge', 'lake tahoe, usa'),
       (7, 'historic castle', 'edinburgh, scotland'),
       (8, 'tropical paradise', 'bali, indonesia'),
       (9, 'business suites', 'tokyo, japan'),
       (10, 'eco-friendly hotel', 'copenhagen, denmark');
```

### Данные для таблицы `room`
```sql
insert into room (id_room, id_hotel, room_type, price, capacity)
values (1, 1, 'single', 150.00, 1),
       (2, 1, 'double', 200.00, 2),
       (3, 1, 'suite', 350.00, 4),
       (4, 2, 'single', 120.00, 1),
       (5, 2, 'double', 180.00, 2),
       (6, 2, 'suite', 300.00, 4),
       (7, 3, 'double', 250.00, 2),
       (8, 3, 'suite', 400.00, 4),
       (9, 4, 'single', 100.00, 1),
       (10, 4, 'double', 150.00, 2),
       (11, 5, 'single', 90.00, 1),
       (12, 5, 'double', 140.00, 2),
       (13, 6, 'suite', 280.00, 4),
       (14, 7, 'double', 220.00, 2),
       (15, 8, 'single', 130.00, 1),
       (16, 8, 'double', 190.00, 2),
       (17, 9, 'suite', 360.00, 4),
       (18, 10, 'single', 110.00, 1),
       (19, 10, 'double', 160.00, 2);
```

### Данные для таблицы `customer`
```sql
insert into customer (id_customer, name, email, phone)
values (1, 'john doe', 'john.doe@example.com', '+1234567890'),
       (2, 'jane smith', 'jane.smith@example.com', '+0987654321'),
       (3, 'alice johnson', 'alice.johnson@example.com', '+1122334455'),
       (4, 'bob brown', 'bob.brown@example.com', '+2233445566'),
       (5, 'charlie white', 'charlie.white@example.com', '+3344556677'),
       (6, 'diana prince', 'diana.prince@example.com', '+4455667788'),
       (7, 'ethan hunt', 'ethan.hunt@example.com', '+5566778899'),
       (8, 'fiona apple', 'fiona.apple@example.com', '+6677889900'),
       (9, 'george washington', 'george.washington@example.com', '+7788990011'),
       (10, 'hannah montana', 'hannah.montana@example.com', '+8899001122');
```

### Данные для таблицы `booking`
```sql
insert into booking (id_booking, id_room, id_customer, check_in_date, check_out_date)
values (1, 1, 1, '2025-05-01', '2025-05-05'),    -- 4 ночи, john doe
       (2, 2, 2, '2025-05-02', '2025-05-06'),    -- 4 ночи, jane smith
       (3, 3, 3, '2025-05-03', '2025-05-07'),    -- 4 ночи, alice johnson
       (4, 4, 4, '2025-05-04', '2025-05-08'),    -- 4 ночи, bob brown
       (5, 5, 5, '2025-05-05', '2025-05-09'),    -- 4 ночи, charlie white
       (6, 6, 6, '2025-05-06', '2025-05-10'),    -- 4 ночи, diana prince
       (7, 7, 7, '2025-05-07', '2025-05-11'),    -- 4 ночи, ethan hunt
       (8, 8, 8, '2025-05-08', '2025-05-12'),    -- 4 ночи, fiona apple
       (9, 9, 9, '2025-05-09', '2025-05-13'),    -- 4 ночи, george washington
       (10, 10, 10, '2025-05-10', '2025-05-14'), -- 4 ночи, hannah montana
       (11, 1, 2, '2025-05-11', '2025-05-15'),   -- 4 ночи, jane smith
       (12, 2, 3, '2025-05-12', '2025-05-14'),   -- 2 ночи, alice johnson
       (13, 3, 4, '2025-05-13', '2025-05-15'),   -- 2 ночи, bob brown
       (14, 4, 5, '2025-05-14', '2025-05-16'),   -- 2 ночи, charlie white
       (15, 5, 6, '2025-05-15', '2025-05-16'),   -- 1 ночь, diana prince
       (16, 6, 7, '2025-05-16', '2025-05-18'),   -- 2 ночи, ethan hunt
       (17, 7, 8, '2025-05-17', '2025-05-21'),   -- 4 ночи, fiona apple
       (18, 8, 9, '2025-05-18', '2025-05-19'),   -- 1 ночь, george washington
       (19, 9, 10, '2025-05-19', '2025-05-22'),  -- 3 ночи, hannah montana
       (20, 10, 1, '2025-05-20', '2025-05-22'),  -- 2 ночи, john doe
       (21, 1, 2, '2025-05-21', '2025-05-23'),   -- 2 ночи, jane smith
       (22, 2, 3, '2025-05-22', '2025-05-25'),   -- 3 ночи, alice johnson
       (23, 3, 4, '2025-05-23', '2025-05-26'),   -- 3 ночи, bob brown
       (24, 4, 5, '2025-05-24', '2025-05-25'),   -- 1 ночь, charlie white
       (25, 5, 6, '2025-05-25', '2025-05-27'),   -- 2 ночи, diana prince
       (26, 6, 7, '2025-05-26', '2025-05-29');   -- 3 ночи, ethan hunt
```

---

# Задачи для решения

* [задача 1](./tasks/task-1.md)
* [задача 2](./tasks/task-2.md)
* [задача 3](./tasks/task-3.md)
