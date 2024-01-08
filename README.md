# sql_hw2
-- Создание таблицы "manufacturer"
DROP TABLE IF EXISTS manufacturer;
CREATE TABLE manufacturer (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50)
);

-- Заполнение данными таблицы "manufacturer"
INSERT INTO manufacturer (name)
VALUES 
('Apple'),
('Samsung'),
('Huawei');

--Вывод статуса количества мобильных телефонов
SELECT 
`product_name`,
`manufacturer`,
CASE 
WHEN product_count<100 THEN 'little'
WHEN product_count between 100 AND 300 THEN 'many'
WHEN product_count>300 THEN ' lots'
ELSE 'unspecified'
END AS `status_count` 
FROM `mobile_phones`;

-- Добавление внешнего ключа в таблицу mobile_phones
ALTER TABLE mobile_phones
ADD COLUMN manufacturer_id BIGINT,
ADD CONSTRAINT fk_manufacturer
    FOREIGN KEY (manufacturer_id)
    REFERENCES manufacturers(id)
    ON UPDATE CASCADE ON DELETE SET NULL;

-- Обновление значения manufacturer_id в таблице mobile_phones на основе поля manufacturer
UPDATE mobile_phones SET manufacturer_id = 1 WHERE manufacturer = 'Apple';
UPDATE mobile_phones SET manufacturer_id = 2 WHERE manufacturer = 'Samsung';
UPDATE mobile_phones SET manufacturer_id = 3 WHERE manufacturer = 'Huawei';

-- Удаление атрибута manufacturer из таблицы mobile_phones
ALTER TABLE mobile_phones
DROP COLUMN manufacturer;

-- Вывод id, product_name и manufacturer_id из таблицы mobile_phones
SELECT id, product_name, manufacturer_id FROM mobile_phones;

--Вывод подробного описания статуса

SELECT 
`id`,
CASE `order_status` 
WHEN 'OPEN' THEN 'Order is in open stat'
WHEN 'CLOSED' THEN 'Order is closed'
WHEN 'CANCELLED' THEN 'Order is cancelled'
ELSE 'unspecified'
END AS `full_order_status` 
FROM `orders`;
