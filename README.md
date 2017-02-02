#В поисках ответов

Спасибо за интересные вопросы.

##Вопрос 1


#####Первый сферический в вакууме

```php
/**
 * Вернет массив номеров автомобилей и их водителей, которые
 * зарегистрированых в гараже с идентификатором $garageId
 *
 * @param int $garageId
 *
 * @return array
 * @throws Exception
 */
public function getVehiclesAndDriversByGarageId(int $garageId) : array
{
    $query = $this->mysqli->query("
        SELECT
            vehicles.license_plate AS vehicle_license_plate,
            drivers.name AS driver_name
        FROM
            vehicles
        LEFT JOIN drivers ON drivers.id = vehicles.driver_id
        WHERE
            vehicles.garage_id = {$garageId}
    ");

    $result = [];

    if ($query) {
        while ($row = $query->fetch_assoc()) {
            $result[] = $row;
        }

        $query->close();
    } else {
        throw new Exception('Ошибка при выполнении запроса' . PHP_EOL .
                            $this->mysqli->connect_error);
    }

    return $result;
}
```

#####Такой же второй
```php
/**
 * Вернет массив водителей старше заданного возраста
 *
 * @param int $age
 *
 * @return array
 * @throws Exception
 */
public function getOldDrivers(int $age) : array
{
    $query = $this->mysqli->query("
        SELECT
            drivers.name AS driver_name
        FROM
            drivers
        WHERE
            drivers.age > {$age}
    ");

    $result = [];

    if ($query) {
        while ($row = $query->fetch_assoc()) {
            $result[] = $row;
        }

        $query->close();
    } else {
        throw new Exception('Ошибка при выполнении запроса' . PHP_EOL .
                            $this->mysqli->connect_error);
    }

    return $result;
}
```

##Вопрос 2
Оператор JOIN используется для объединения строк двух или более таблиц.

#####пример
```sql
SELECT
	table1.title,
	table2.model
FROM
	table1
LEFT JOIN table2 ON table1.id = table2.table1_id
```

#####пример
```sql
SELECT
	table1.title,
	table2.model,
	table3.address
FROM
	table1
INNER JOIN table2 ON table1.id = table2.table1_id
LEFT JOIN table3 ON table2.id = table3.table2_id
```

Виды: 
INNER JOIN: возвращает только пересечения обоих таблиц  
LEFT JOIN: возвращает все значения левой и пересечения правой  
RIGHT JOIN: возвращает все значения правой и пересечения левой  
FULL JOIN: возвращает все значения обоих таблиц    

На мой взгляд join нужно использовать всегда, когда это необходимо. Ну и join стоит использовать по колонкам с индексом, но это справедливо для любой выборки по условию.

##Вопрос 3
Memcached - это программа создющая key-value хранилище в оперативной памяти. Из-за своей высокой скорости отлично справляется с кэшированием данных, генерация которых требует большого количества ресурсов, хотя и не только. Подводные камни технологии связаны с некоторыми ограничениями, (например объем записи 1Мб), и тем, что это key-value хранилище)). К сожалению кокретно проблемы реализации указать не могу.

##Вопрос 4
Возможные способы выставления cookies
```php
setcookie('cookieName', 'cookieValue', strtotime('+1 week'), '/', 'domain.ru', false, true);
setcookie('cookieName', 'cookieValue', strtotime('+1 week'), '/', 'domain.ru');

header('Set-Cookie: cookieName=cookieValue; Domain=domain.ru; Path=/; Expires=Tue, 28 Feb 2017 00:00:00 GMT; HttpOnly');
```

##Вопрос 5
strlen    = O(1) алгоритм берет и возвращает информацию о занимаемой объектом памяти  
mb_strlen = O(n) алгоритм содержит цикл с итерацией на каждую букву

##Вопрос 6
Например приложение будет реализовано, как SPA на angular, которое общается через REST API с нашим бекендом. Приложение формирует модель данных поисковых фильтров get запросом получает максимальный, минимальный возраст и вес, варианты полов и города. После нажатия кнопки "поиск", отправляется get запрос с поисковыми данными, получается ответ отправляется в модель и отрисовывается таблица с результатами поиска и пагинация(если необходима), появляются кнопки сортировки по дате добавления и возрасту, которые также отправляют get запросы с поисковыми данными и условием сортировки. 

