# Схема базы данных транспортных средств

![vehicle-db-diagram](../imgs/vehicle-db-diagram.png)

---

# Скрипты создания таблиц

### Таблица `vehicle`
```sql
create table vehicle (
    maker   varchar(100)    not null,
    model   varchar(100)    not null,
    type    varchar(20)     not null check (type in ('car', 'motorcycle', 'bicycle')),
    primary key (model)
);
```

### Таблица `car`
```sql
create table car (
    vin             varchar(17)     not null,
    model           varchar(100)    not null,
    engine_capacity decimal(4, 2)   not null, -- объем двигателя в литрах
    horsepower      int             not null, -- мощность в лошадиных силах
    price           decimal(10, 2)  not null, -- цена в долларах
    transmission    varchar(20)     not null check (transmission in ('automatic', 'manual')), -- тип трансмиссии
    primary key (vin),
    foreign key (model) references vehicle(model)
);
```

### Таблица `motorcycle`
```sql
create table motorcycle (
    vin             varchar(17)     not null,
    model           varchar(100)    not null,
    engine_capacity decimal(4, 2)   not null, -- объем двигателя в литрах
    horsepower      int             not null, -- мощность в лошадиных силах
    price           decimal(10, 2)  not null, -- цена в долларах
    type            varchar(20)     not null check (type in ('sport', 'cruiser', 'touring')), -- тип мотоцикла
    primary key (vin),
    foreign key (model) references vehicle(model)
);
```

### Таблица `bicycle`
```sql
create table bicycle (
    serial_number   varchar(20)     not null,
    model           varchar(100)    not null,
    gear_count      int             not null, -- количество передач
    price           decimal(10, 2)  not null, -- цена в долларах
    type            varchar(20)     not null check (type in ('mountain', 'road', 'hybrid')), -- тип велосипеда
    primary key (serial_number),
    foreign key (model) references vehicle(model)
);
```

---

# Скрипты наполнения данными

### Данные для таблицы `vehicle`
```sql
insert into vehicle (maker, model, type)
values ('toyota', 'camry', 'car'),
       ('honda', 'civic', 'car'),
       ('ford', 'mustang', 'car'),
       ('yamaha', 'yzf-r1', 'motorcycle'),
       ('harley-davidson', 'sportster', 'motorcycle'),
       ('kawasaki', 'ninja', 'motorcycle'),
       ('trek', 'domane', 'bicycle'),
       ('giant', 'defy', 'bicycle'),
       ('specialized', 'stumpjumper', 'bicycle');
```

### Данные для таблицы `car`
```sql
insert into car (vin, model, engine_capacity, horsepower, price, transmission)
values ('1hgcm82633a123456', 'camry', 2.5, 203, 24000.00, 'automatic'),
       ('2hgfg3b53gh123456', 'civic', 2.0, 158, 22000.00, 'manual'),
       ('1fa6p8cf0j1234567', 'mustang', 5.0, 450, 55000.00, 'automatic');
```

### Данные для таблицы `motorcycle`
```sql
insert into motorcycle (vin, model, engine_capacity, horsepower, price, type)
values ('jyarn28e9fa123456', 'yzf-r1', 1.0, 200, 17000.00, 'sport'),
       ('1hd1zk3158k123456', 'sportster', 1.2, 70, 12000.00, 'cruiser'),
       ('jkbvnaf156a123456', 'ninja', 0.9, 150, 14000.00, 'sport');
```

### Данные для таблицы `bicycle`
```sql
insert into bicycle (serial_number, model, gear_count, price, type)
values ('sn123456789', 'domane', 22, 3500.00, 'road'),
       ('sn987654321', 'defy', 20, 3000.00, 'road'),
       ('sn456789123', 'stumpjumper', 30, 4000.00, 'mountain');
```

---

# Задачи для решения

* [задача 1](./tasks/task-1.md)
* [задача 2](./tasks/task-2.md)
