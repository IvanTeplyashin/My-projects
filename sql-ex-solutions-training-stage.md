### Решение задач блока SELECT(обучающий этап) с сайта sql-ex.ru
##### Задача 1
> Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd
```SQL
SELECT model, speed, hd
FROM PC
WHERE price < 500
```
##### Задача 2
> Найдите производителей принтеров. Вывести: maker
```SQL
SELECT DISTINCT maker
FROM PRODUCT
WHERE type = 'printer'
```
##### Задача 3
> Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.
```SQL
SELECT model, ram, screen
FROM Laptop
WHERE price > 1000
```
##### Задача 4
> Найдите все записи таблицы Printer для цветных принтеров.
```SQL
SELECT code, model, color, type, price
FROM Printer
WHERE color = 'y'
```
##### Задача 5
> Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.
```SQL
SELECT model, speed, hd
FROM PC
WHERE (cd = '12x' OR cd = '24x') AND price < 600
```
##### Задача 6
> Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость.
```SQL
SELECT DISTINCT Product.maker, Laptop.speed
FROM Product, Laptop
WHERE Product.model = Laptop.model 
      and Laptop.hd >= 10
```
##### Задача 7
> Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква).
```SQL
SELECT * FROM (SELECT model, price FROM PC
         UNION SELECT model, price FROM Laptop
         UNION SELECT model, price FROM Printer) as dop
WHERE dop.model IN (SELECT model FROM Product WHERE Product.maker = 'b')
```
##### Задача 8
> Найдите производителя, выпускающего ПК, но не ПК-блокноты.
```SQL
SELECT DISTINCT maker
FROM Product
WHERE type = 'PC'
AND maker NOT IN (SELECT maker FROM Product WHERE type = 'Laptop')
```
##### Задача 9
> Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker
```SQL
SELECT DISTINCT maker
FROM Product, PC
WHERE Product.model = PC.model AND PC.speed >= 450
```
##### Задача 10
> Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price
```SQL
SELECT model, price
FROM Printer
WHERE price = (SELECT MAX(PRICE)
               FROM Printer)
```
##### Задача 11
> Найдите среднюю скорость ПК.
```SQL
SELECT AVG(speed) 
FROM PC
```
##### Задача 12
> Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.
```SQL
SELECT AVG(speed)
FROM Laptop
WHERE price > 1000
```
##### Задача 13
> Найдите среднюю скорость ПК, выпущенных производителем A.
```SQL
Select AVG(speed)
FROM PC
WHERE model IN (SELECT model FROM Product WHERE maker = 'A')
```
##### Задача 14
> Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий.
```SQL
SELECT Ships.class, Ships.name, Classes.country
FROM Ships, Classes
WHERE Ships.Class = Classes.Class
      AND Classes.numGuns >= 10
```
##### Задача 15
> Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD
```SQL
SELECT hd
FROM PC 
GROUP BY hd
HAVING COUNT(hd) >= 2
```
##### Задача 16
> Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM.
```SQL
SELECT DISTINCT A.model, B.model, B.speed, B.ram
FROM PC AS A, PC AS B
WHERE A.speed = B.speed AND A.ram = B.ram AND A.model > B.model
```
##### Задача 17
> Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК.
Вывести: type, model, speed
```SQL
SELECT DISTINCT Product.type, Laptop.model, Laptop.speed
FROM Product, Laptop
WHERE Product.type = 'Laptop' AND Laptop.speed < ALL (SELECT speed FROM PC)
```
##### Задача 18
> Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price
```SQL
SELECT DISTINCT c.maker, a.priceA price
FROM (SELECT MIN(price) priceA
      FROM Printer
      WHERE Color ='y'
      ) a 
    INNER JOIN (SELECT model, price FROM Printer WHERE Color = 'y') b ON a.priceA = b.price 
INNER JOIN Product c ON b.model = c.model
```
##### Задача 19
> Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов.
Вывести: maker, средний размер экрана.
```SQL
SELECT b.maker, AVG(a.screen)
FROM (SELECT model, screen
      FROM Laptop) a
INNER JOIN Product b ON a.model = b.model
GROUP BY b.maker
```
##### Задача 20
> Найдите производителей, выпускающих по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК.
```SQL
SELECT maker, COUNT(model)
FROM Product
WHERE type = 'PC' 
GROUP BY maker
HAVING COUNT(model) >= 3
```
##### Задача 21
> Найдите максимальную цену ПК, выпускаемых каждым производителем, у которого есть модели в таблице PC.
Вывести: maker, максимальная цена.
```SQL
SELECT maker, MAX(price) max_price
FROM Product
INNER JOIN (SELECT model, price FROM PC) a ON Product.model = a.model
GROUP BY maker
```
##### Задача 22
> Для каждого значения скорости ПК, превышающего 600 МГц, определите среднюю цену ПК с такой же скоростью. Вывести: speed, средняя цена.
```SQL
SELECT speed, AVG(price) avg_price  
FROM PC
WHERE speed > 600
GROUP BY speed
```
##### Задача 23
> Найдите производителей, которые производили бы как ПК
со скоростью не менее 750 МГц, так и ПК-блокноты со скоростью не менее 750 МГц.
Вывести: Maker
```SQL
SELECT с.maker
FROM Product с
INNER JOIN (SELECT model FROM PC WHERE speed >= 750) a ON с.model = a.model 
INTERSECT
SELECT d.maker
FROM Product d
INNER JOIN (SELECT model FROM Laptop WHERE speed >= 750) b ON d.model = b.model
```
##### Задача 24
> Перечислите номера моделей любых типов, имеющих самую высокую цену по всей имеющейся в базе данных продукции.
```SQL
WITH max_out
AS (
SELECT model, price
FROM PC
UNION ALL
SELECT model, price
FROM Laptop
UNION ALL
SELECT model, price
FROM Printer
)
SELECT DISTINCT model
FROM max_out
WHERE price >= (SELECT MAX(price) FROM max_out)
```
##### Задача 25
> Найдите производителей принтеров, которые производят ПК с наименьшим объемом RAM и с самым быстрым процессором среди всех ПК, имеющих наименьший объем RAM. Вывести: Maker
```SQL
SELECT DISTINCT maker
FROM Product
INNER JOIN 
   (SELECT model 
   FROM PC 
   WHERE ram IN (SELECT MIN(ram) FROM PC)
   AND speed IN (SELECT MAX(speed) FROM PC WHERE ram IN (SELECT MIN(ram) FROM PC))
) a 
ON Product.model = a.model
INTERSECT
SELECT DISTINCT maker
FROM Product
WHERE type = 'Printer'
```
##### Задача 26
> Найдите среднюю цену ПК и ПК-блокнотов, выпущенных производителем A (латинская буква). Вывести: одна общая средняя цена.
```SQL
SELECT AVG(a.price) avg_price
FROM (
SELECT price
FROM (SELECT model FROM Product WHERE maker = 'A') b
INNER JOIN PC ON b.model = PC.model
UNION ALL
SELECT price
FROM (SELECT model FROM Product WHERE maker = 'A') c
INNER JOIN LAPTOP ON c.model = LAPTOP.model
) a
```
##### Задача 27
> Найдите средний размер диска ПК каждого из тех производителей, которые выпускают и принтеры. Вывести: maker, средний размер HD.
```SQL
SELECT maker, AVG(hd) avg_hd
FROM (
SELECT model, hd
FROM PC) c
INNER JOIN (SELECT maker, model FROM PRODUCT WHERE type = 'PC') b
ON c.model = b.model
WHERE maker IN (SELECT maker FROM PRODUCT WHERE type = 'Printer')
GROUP BY maker
```
##### Задача 28
> Используя таблицу Product, определить количество производителей, выпускающих по одной модели.
```SQL
SELECT COUNT(a.maker)
FROM (
SELECT maker
FROM Product
GROUP BY maker
HAVING COUNT(model) = 1
) a
```
##### Задача 29
> В предположении, что приход и расход денег на каждом пункте приема фиксируется не чаще одного раза в день [т.е. первичный ключ (пункт, дата)], написать запрос с выходными данными (пункт, дата, приход, расход). Использовать таблицы Income_o и Outcome_o.
```SQL
SELECT p1 AS Point, d1 AS Date, inc, out
FROM (SELECT point AS p1, date AS d1, inc FROM Income_o) a LEFT JOIN (SELECT point AS p2, date AS d2, out FROM Outcome_o) b ON a.p1 = b.p2 AND a.d1 = b.d2
UNION
SELECT p1 AS Point, d1 AS Date, inc, out
FROM (SELECT point AS p1, date AS d1, inc FROM Income_o) a INNER JOIN (SELECT point AS p2, date AS d2, out FROM Outcome_o) b ON a.p1 = b.p2 AND a.d1 = b.d2
UNION
SELECT p1 AS Point, d1 AS Date, inc, out
FROM (SELECT point AS p1, date AS d1, out FROM Outcome_o) a LEFT JOIN (SELECT point AS p2, date AS d2, inc FROM Income_o) b ON a.p1 = b.p2 AND a.d1 = b.d2
```
##### Задача 30
> 
```SQL

```
##### Задача 31
> Для классов кораблей, калибр орудий которых не менее 16 дюймов, укажите класс и страну.
```SQL
SELECT class, country
FROM Classes
WHERE bore >= 16
```
##### Задача 32
> Одной из характеристик корабля является половина куба калибра его главных орудий (mw). С точностью до 2 десятичных знаков определите среднее значение mw для кораблей каждой страны, у которой есть корабли в базе данных.
```SQL
SELECT country, 
cast(AVG(POWER(bore,3)/2) AS DECIMAL(18,2)) AS weight 
FROM (SELECT country, bore, name
FROM Ships INNER JOIN Classes ON Ships.class = Classes.class
UNION
SELECT country, bore, ship
FROM Outcomes INNER JOIN Classes ON Outcomes.ship = Classes.class AND Outcomes.ship NOT IN (SELECT name FROM Ships)) a
GROUP BY country
```
##### Задача 33
> Укажите корабли, потопленные в сражениях в Северной Атлантике (North Atlantic). Вывод: ship.
```SQL
SELECT ship
FROM Outcomes
WHERE battle = 'North Atlantic' AND result = 'sunk'
```
##### Задача 34
> По Вашингтонскому международному договору от начала 1922 г. запрещалось строить линейные корабли водоизмещением более 35 тыс.тонн. Укажите корабли, нарушившие этот договор (учитывать только корабли c известным годом спуска на воду). Вывести названия кораблей.
```SQL
SELECT DISTINCT a.name
FROM (
     SELECT Classes.class, Classes.type, Classes.displacement, Ships.name, Ships.launched
     FROM Classes
     INNER JOIN Ships ON Classes.class = Ships.class
) a
WHERE a.launched >= 1922 AND a.type = 'bb' AND a.displacement > 35000
```
##### Задача 35
> В таблице Product найти модели, которые состоят только из цифр или только из латинских букв (A-Z, без учета регистра).
Вывод: номер модели, тип модели.
```SQL
SELECT model, type
FROM Product
WHERE upper(model) NOT LIKE '%[^A-Z]%'
OR model NOT LIKE '%[^0-9]%'
```
##### Задача 36
> Перечислите названия головных кораблей, имеющихся в базе данных (учесть корабли в Outcomes).
```SQL
SELECT DISTINCT class 
FROM Classes
WHERE class IN(
SELECT DISTINCT name
FROM ships
UNION
SELECT DISTINCT ship
FROM OUTCOMES)
```
##### Задача 37
> Найдите классы, в которые входит только один корабль из базы данных (учесть также корабли в Outcomes).
```SQL
SELECT class
FROM (
SELECT class, ship_name
FROM (SELECT Classes.class, Ships.name AS ship_name FROM Classes, Ships WHERE Classes.class = Ships.class) a
UNION
SELECT class, ship_name
FROM (SELECT Classes.class, Outcomes.ship AS ship_name FROM Classes, Outcomes WHERE Classes.class = Outcomes.ship) b
) e
GROUP BY class
HAVING COUNT(class) = 1
```
##### Задача 38
> Найдите страны, имевшие когда-либо классы обычных боевых кораблей ('bb') и имевшие когда-либо классы крейсеров ('bc').
```SQL
SELECT DISTINCT country
FROM Classes
WHERE type = 'bb'
INTERSECT
SELECT DISTINCT country
FROM Classes
WHERE type = 'bc'
```
##### Задача 39
> 
```SQL

```
##### Задача 40
> Найти производителей, которые выпускают более одной модели, при этом все выпускаемые производителем модели являются продуктами одного типа.
Вывести: maker, type
```SQL
SELECT DISTINCT a.maker, b.type
FROM (SELECT maker
FROM Product
GROUP BY maker
HAVING COUNT(model) > 1 AND COUNT(DISTINCT type) = 1) a
INNER JOIN
(SELECT maker, type
FROM Product) b
ON a.maker = b.maker
```
##### Задача 41
> Для каждого производителя, у которого присутствуют модели хотя бы в одной из таблиц PC, Laptop или Printer,
определить максимальную цену на его продукцию.
Вывод: имя производителя, если среди цен на продукцию данного производителя присутствует NULL, то выводить для этого производителя NULL,
иначе максимальную цену.
```SQL
WITH all_price
AS(
SELECT Product.maker, PC.price
FROM Product, PC
WHERE Product.model = PC.model
UNION 
SELECT Product.maker, Laptop.price
FROM Product, Laptop
WHERE Product.model = Laptop.model
UNION 
SELECT Product.maker, Printer.price
FROM Product, Printer
WHERE Product.model = Printer.model
)
SELECT DISTINCT maker,
CASE WHEN(
FIRST_VALUE(price) OVER 
(PARTITION BY maker ORDER BY price ASC)
) IS NOT NULL
THEN 
FIRST_VALUE(price) OVER 
(PARTITION BY maker ORDER BY price DESC)
ELSE NULL
END AS max_price
FROM all_price
```
##### Задача 42
> Найдите названия кораблей, потопленных в сражениях, и название сражения, в котором они были потоплены.
```SQL
SELECT ship, battle
FROM OUTCOMES
WHERE result = 'sunk'
```
##### Задача 43
> Укажите сражения, которые произошли в годы, не совпадающие ни с одним из годов спуска кораблей на воду.
```SQL
SELECT name
FROM Battles
WHERE DATEPART(yyyy, date)
NOT IN(SELECT launched FROM Ships WHERE launched IS NOT NULL)
```
##### Задача 44
> Найдите названия всех кораблей в базе данных, начинающихся с буквы R.
```SQL
SELECT name
FROM Ships
WHERE name LIKE 'R%'
UNION
SELECT ship
FROM Outcomes
WHERE ship LIKE 'R%'
```
##### Задача 45
> Найдите названия всех кораблей в базе данных, состоящие из трех и более слов (например, King George V).
Считать, что слова в названиях разделяются единичными пробелами, и нет концевых пробелов.
```SQL
SELECT name
FROM Ships
WHERE name LIKE '% % %'
UNION
SELECT ship
FROM Outcomes
WHERE ship LIKE '% % %'
```
##### Задача 46
> Для каждого корабля, участвовавшего в сражении при Гвадалканале (Guadalcanal), вывести название, водоизмещение и число орудий.
```SQL
SELECT ship, displacement, numGuns
FROM (
SELECT ship 
FROM Outcomes
WHERE battle = 'Guadalcanal') a
LEFT JOIN (SELECT name, class FROM ships) b ON a.ship = b.name
LEFT JOIN (SELECT class, displacement, numGuns FROM Classes) c ON b.class = c.class OR a.ship = c.class
```
##### Задача 47
> Определить страны, которые потеряли в сражениях все свои корабли.
```SQL
SELECT DISTINCT country
FROM (SELECT country
FROM Ships INNER JOIN Classes ON Ships.class = Classes.class
UNION
SELECT country
FROM Outcomes INNER JOIN Classes ON Outcomes.ship = Classes.class AND Outcomes.ship NOT IN (SELECT name FROM Ships)) b
WHERE country NOT IN(
SELECT DISTINCT country
FROM (SELECT country, name
FROM Ships INNER JOIN Classes ON Ships.class = Classes.class
UNION
SELECT country, ship
FROM Outcomes INNER JOIN Classes ON Outcomes.ship = Classes.class AND Outcomes.ship NOT IN (SELECT name FROM Ships)) a
WHERE name NOT IN (SELECT ship FROM Outcomes WHERE result = 'sunk'))
```
##### Задача 48
> Найдите классы кораблей, в которых хотя бы один корабль был потоплен в сражении.
```SQL
SELECT Classes.class
FROM Ships INNER JOIN Classes ON Ships.class = Classes.class AND Ships.name IN (SELECT ship FROM Outcomes WHERE result = 'sunk')
UNION
SELECT Classes.class
FROM Outcomes INNER JOIN Classes ON Outcomes.ship = Classes.class AND Outcomes.ship NOT IN (SELECT name FROM Ships) AND Outcomes.ship IN (SELECT ship FROM Outcomes WHERE result = 'sunk')
```
##### Задача 49
> Найдите названия кораблей с орудиями калибра 16 дюймов (учесть корабли из таблицы Outcomes).
```SQL
SELECT name
FROM Ships INNER JOIN Classes ON Ships.class = Classes.class
WHERE bore = 16
UNION 
SELECT ship
FROM Outcomes INNER JOIN Classes ON Outcomes.ship = Classes.class
WHERE bore = 16
```
##### Задача 50
> Найдите сражения, в которых участвовали корабли класса Kongo из таблицы Ships.
```SQL
SELECT DISTINCT battle
FROM
Battles a
INNER JOIN Outcomes b 
ON a.name = b.battle
INNER JOIN Ships c
ON b.ship = c.name
WHERE class = 'Kongo'
```
##### Задача 51
> 
```SQL

```
##### Задача 52
> Определить названия всех кораблей из таблицы Ships, которые могут быть линейным японским кораблем,
имеющим число главных орудий не менее девяти, калибр орудий менее 19 дюймов и водоизмещение не более 65 тыс.тонн
```SQL
SELECT name
FROM Ships a
LEFT JOIN 
Classes b ON a.class = b.class
WHERE (b.type = 'bb' OR b.type IS NULL) 
AND (b.country = 'JAPAN' OR b.country IS NULL) 
AND (b.numGuns >=9 OR b.numGuns IS NULL) AND (b.bore < 19 OR b.bore IS NULL) 
AND (b.displacement <= 65000 OR b.displacement IS NULL)
```
##### Задача 53
> 
```SQL

```
##### Задача 54
> 
```SQL

```
##### Задача 55
> 
```SQL

```
##### Задача 56
> 
```SQL

```
##### Задача 57
> 
```SQL

```
##### Задача 58
> 
```SQL

```
##### Задача 59
> Посчитать остаток денежных средств на каждом пункте приема для базы данных с отчетностью не чаще одного раза в день. Вывод: пункт, остаток.
```SQL
SELECT point, SUM(rem)
FROM(
SELECT point, inc AS rem 
FROM Income_o
UNION ALL
SELECT point, (-1)*out AS rem
FROM Outcome_o
) a
GROUP BY(point)
```
##### Задача 60
> Посчитать остаток денежных средств на начало дня 15/04/2001 на каждом пункте приема для базы данных с отчетностью не чаще одного раза в день. Вывод: пункт, остаток.
Замечание. Не учитывать пункты, информации о которых нет до указанной даты.
```SQL
SELECT point, SUM(rem) AS Remains
FROM(
SELECT point, inc AS rem 
FROM Income_o
WHERE date < '20010415'
UNION ALL
SELECT point, (-1)*out AS rem
FROM Outcome_o
WHERE date < '20010415'
) a
GROUP BY(point)
```
##### Задача 61
> Посчитать остаток денежных средств на всех пунктах приема для базы данных с отчетностью не чаще одного раза в день.
```SQL
SELECT i.sum - o.sum AS Remain
FROM 
(SELECT ISNULL(SUM(inc),0) AS sum FROM Income_o) i, 
(SELECT ISNULL(SUM(out),0) AS sum FROM Outcome_o) o
```
##### Задача 62
> 
```SQL

```
##### Задача 63
> 
```SQL

```
##### Задача 64
> 
```SQL

```
##### Задача 65
> Пронумеровать уникальные пары {maker, type} из Product, упорядочив их следующим образом:
- имя производителя (maker) по возрастанию;
- тип продукта (type) в порядке PC, Laptop, Printer.
Если некий производитель выпускает несколько типов продукции, то выводить его имя только в первой строке;
остальные строки для ЭТОГО производителя должны содержать пустую строку символов ('').
```SQL
SELECT row_number() OVER 
(ORDER BY maker, 
CASE WHEN type = 'PC' 
THEN 0 
WHEN type = 'Laptop' 
THEN 1 
ELSE 2 
END), CASE WHEN rn = 1 THEN maker ELSE '' END, type
FROM (
SELECT maker, type,
ROW_NUMBER() OVER (PARTITION BY maker ORDER BY CASE WHEN type = 'PC' 
THEN 0 
WHEN type = 'Laptop' 
THEN 1 
ELSE 2 
END) AS rn 
  FROM Product
  GROUP BY maker, type
) a
```
##### Задача 66
> Для всех дней в интервале с 01/04/2003 по 07/04/2003 определить число рейсов из Rostov с пассажирами на борту.
Вывод: дата, количество рейсов.
```SQL
SELECT Date, SUM(qty) AS QTY
FROM (Select CAST(date AS DATETIME) AS Date, COUNT (DISTINCT trip_no) AS qty
FROM 
(SELECT trip_no, date, ID_psg FROM Pass_in_trip) a
INNER JOIN
(SELECT DISTINCT trip_no AS num, town_from FROM Trip) b
ON a.trip_no = b.num
WHERE YEAR(date) = 2003 AND MONTH(date) = 4  AND town_from = 'Rostov' AND DAY(date) IN (1, 2, 3, 4, 5, 6, 7)
GROUP BY trip_no, date
HAVING COUNT(ID_psg) > 0
UNION ALL 
SELECT CAST('2003-04-01 00:00:00.000' AS DATETIME) AS date,
0 AS qty
UNION ALL
SELECT CAST('2003-04-02 00:00:00.000' AS DATETIME) AS Date,
0 AS qty
UNION ALL
SELECT CAST('2003-04-03 00:00:00.000' AS DATETIME) AS Date,
0 AS qty
UNION ALL
SELECT CAST('2003-04-04 00:00:00.000' AS DATETIME) AS Date,
0 AS qty
UNION ALL
SELECT CAST('2003-04-05 00:00:00.000' AS DATETIME) AS Date,
0 AS qty
UNION ALL
SELECT CAST('2003-04-06 00:00:00.000' AS DATETIME) AS Date,
0 AS qty
UNION ALL
SELECT CAST('2003-04-07 00:00:00.000' AS DATETIME) AS Date,
0 AS qty) s
GROUP BY date

```
##### Задача 67
> 
```SQL

```
##### Задача 68
> 
```SQL

```
##### Задача 69
> 
```SQL

```
##### Задача 70
> 
```SQL

```
##### Задача 71
> 
```SQL

```
##### Задача 72
> 
```SQL

```
##### Задача 73
> Для каждой страны определить сражения, в которых не участвовали корабли данной страны.
Вывод: страна, сражение
```SQL
SELECT Classes.country, Battles.name AS battle
FROM Classes, Battles
EXCEPT
(SELECT country, battle 
FROM Ships INNER JOIN Classes ON Ships.class = Classes.class INNER JOIN Outcomes ON Ships.name = Outcomes.ship
UNION
SELECT country, battle
FROM Outcomes INNER JOIN Classes ON Outcomes.ship = Classes.class AND Outcomes.ship NOT IN (SELECT name FROM Ships))
```
##### Задача 74
> 
```SQL

```
##### Задача 75
> Для тех производителей, у которых есть продукты с известной ценой хотя бы в одной из таблиц Laptop, PC, Printer найти максимальные цены на каждый из типов продукции.
Вывод: maker, максимальная цена на ноутбуки, максимальная цена на ПК, максимальная цена на принтеры.
Для отсутствующих продуктов/цен использовать NULL.
```SQL
SELECT DISTINCT maker, max_price_laptop, max_price_pc, max_price_printer
FROM
(SELECT DISTINCT maker, 
FIRST_VALUE(price) OVER 
(PARTITION BY maker ORDER BY price DESC) AS max_price_laptop
FROM (SELECT maker, price FROM Product LEFT JOIN Laptop ON Product.model = Laptop.model) a) a0
LEFT JOIN
(SELECT DISTINCT maker AS mp, 
FIRST_VALUE(price) OVER 
(PARTITION BY maker ORDER BY price DESC) AS max_price_pc
FROM (SELECT maker, price FROM Product LEFT JOIN PC ON Product.model = PC.model) b) b0
ON a0.maker = b0.mp
LEFT JOIN
(SELECT DISTINCT maker AS mpr, 
FIRST_VALUE(price) OVER 
(PARTITION BY maker ORDER BY price DESC) AS max_price_printer
FROM (SELECT maker, price FROM Product LEFT JOIN Printer ON Product.model = Printer.model) c) c0
ON a0.maker = c0.mpr
WHERE max_price_laptop IS NOT NULL OR max_price_pc IS NOT NULL OR max_price_printer IS NOT NULL
```
##### Задача 76
> 
```SQL

```
##### Задача 77
> 
```SQL

```
##### Задача 78
> Для каждого сражения определить первый и последний день
месяца,
в котором оно состоялось.
Вывод: сражение, первый день месяца, последний
день месяца.

Замечание: даты представить без времени в формате "yyyy-mm-dd".
```SQL
SELECT name, DATEFROMPARTS(YEAR(date), MONTH(date), 1) ,EOMONTH(date) AS last_day
FROM Battles
```
##### Задача 79
> 
```SQL

```
##### Задача 80
> Найти производителей любой компьютерной техники, у которых нет моделей ПК, не представленных в таблице PC.
```SQL
SELECT DISTINCT maker 
FROM Product
EXCEPT
SELECT DISTINCT maker 
FROM Product
WHERE model IN
(SELECT DISTINCT model
FROM Product
WHERE type = 'PC'
EXCEPT 
SELECT DISTINCT model
FROM PC)
```
##### Задача 81
> 
```SQL

```
##### Задача 82
> 
```SQL

```
##### Задача 83
> 
```SQL

```
##### Задача 84
> 
```SQL

```
##### Задача 85
> 
```SQL

```
##### Задача 86
> 
```SQL

```
##### Задача 87
> 
```SQL

```
##### Задача 88
> 
```SQL

```
##### Задача 89
> 
```SQL

```
##### Задача 90
> 
```SQL

```
##### Задача 91
> 
```SQL

```
##### Задача 92
> 
```SQL

```

