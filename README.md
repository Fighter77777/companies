# Задание № 1

#### A. Вернуть название фирмы и ее телефон. В результате должны быть представлены         все фирмы по одному разу. Если у фирмы нет телефона, нужно вернуть пробел или прочерк. Если у фирмы несколько телефонов, нужно вернуть любой из них.
```
SELECT `firm`.`name`, COALESCE(`phone`.`phone`, '-') `phone`
FROM `firm`
LEFT JOIN `phone` ON `firm`.`id` = `phone`.`firm_id`
GROUP BY `firm`.`id`
```
![presentation 1.a](http://dl4.joxi.net/drive/2020/09/17/0026/1186/1746082/82/62f1e64e61.jpg)

#### B. Вернуть все фирмы, не имеющие телефонов.
```
SELECT `firm`.`name`
FROM `firm`
LEFT JOIN `phone` ON `firm`.`id` = `phone`.`firm_id`
WHERE `phone`.`id` IS NULL
```
![presentation 1.b](http://dl4.joxi.net/drive/2020/09/17/0026/1186/1746082/82/825515049e.jpg)

#### C. Вернуть все фирмы, имеющие не менее 2-х телефонов.
```
SELECT `firm`.`name`
FROM `firm`
INNER JOIN `phone` ON `firm`.`id` = `phone`.`firm_id`
GROUP BY `firm`.`id`
HAVING COUNT(`phone`.`id`) >= 2
```
![presentation 1.c](http://dl4.joxi.net/drive/2020/09/17/0026/1186/1746082/82/44a75d8857.jpg)

#### D. Вернуть все фирмы, имеющие менее 2-х телефонов.
```
SELECT `firm`.`name`
FROM `firm`
LEFT JOIN `phone` ON `firm`.`id` = `phone`.`firm_id`
GROUP BY `firm`.`id`
HAVING COUNT(`phone`.`id`) < 2
```
![presentation 1.d](http://joxi.ru/LmGM9ZoHleJMNA.jpg)

#### E. Вернуть фирму, имеющую максимальное кол-во телефонов.
```
SELECT `firm`.`name`
FROM `firm`
INNER JOIN `phone` ON `firm`.`id` = `phone`.`firm_id`
GROUP BY `firm`.`id`
ORDER BY COUNT(`phone`.`id`) DESC
LIMIT 1
```
![presentation 1.e](http://dl3.joxi.net/drive/2020/09/17/0026/1186/1746082/82/e2d72c4af2.jpg)


# Задание № 2
#### A. Вывести общий объем поставок каждого из продуктов для каждой фирмы с указанием даты последней поставки
```
SELECT `company`.`name` `company name`, `goods`.`name` `good name`, COUNT(`goods`.`id`) `amount`, MAX(`shipdate`) `last shipment`
FROM  `shipment`
INNER JOIN  `company` ON `company`.`id` = `shipment`.`company_id`
INNER JOIN  `goods`  ON `goods`.`id` = `shipment`.`good_id`
GROUP BY `goods`.`id`, `company`.`id`
```
![presentation 2.a](http://dl3.joxi.net/drive/2020/09/17/0026/1186/1746082/82/46a44cdff1.jpg)

#### B. Аналогично предыдущему пункту, но за последние 30 дней. Если поставки               какого-либо из товаров для компании в этот период отсутствовали, вывести в столбце объема 'No data'
```
SELECT
  `goods`.`name` `good name`,
  `company`.`name` `company name`,
  IF(COUNT(`shipment`.`id`), COUNT(`shipment`.`id`), 'No data' ) `amount`,
  MAX(`shipdate`)`last shipment`
FROM `goods`
LEFT JOIN `shipment` ON `goods`.`id` = `shipment`.`good_id` AND `shipdate` >= DATE_SUB(CURDATE( ), INTERVAL 30 DAY)
LEFT JOIN `company` ON `company`.`id` = `shipment`.`company_id`
GROUP BY`goods`.`id`, `company`.`id`
```
![presentation 2.b](http://dl3.joxi.net/drive/2020/09/17/0026/1186/1746082/82/e4d96fcb39.jpg)
