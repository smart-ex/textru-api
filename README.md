# Описание
PHP коннектор для API сайта text.ru.
Легко интегрируется в основные php фреймворки (протестирован на Laravel).

## Установка
composer install textru-api

## Описание API
Используется POST версия API от text.ru, более подробную информацию об API можно найти по ссылке:
https://text.ru/api-check/manual

## Примеры
Реализовано два варианта использования компонента, с созданием экземпляра класса и без него (через статические методы).

### Вариант 1. С созданием экземпляра класса
Способ удобен если у вас один аккаунт на text.ru
Первым делом создаете экземпляр класса, передав в него свой [userkey](https://text.ru/api-check)

```php
$userkey = 'Ваш text.ru userkey';
$text = 'Проверяемый текст, не менее 100 символов';

$app = new \TextRuApi\TextRuApi($userkey);

//Добавляете текст на проверку и сохраняете text_uid для последующего получения результатов
$options = ["exceptdomain"=>"mydomain.ru"]; //Необязательный параметр. Массив дополнительных параметров (см. описание API)
$result = TextRuApi->add($text, $options);
$uid = $result["text_uid"];

//Требуется выждать паузу чтобы сервис успел обработать текст.
//Рекомендуется больше минуты.
sleep(15);

//Получаете результат проверки
$jsonvisible = 'detail'; //Необязательный параметр. Укажите "detail" чтобы получить расширенные данные по тексту
$result = TextRuApi->get($uid, $jsonvisible);
```

### Вариант 2. Без создания экземпляра класса
Можно просто использовать методы как статические, каждый раз передавая в них ваш [userkey](https://text.ru/api-check)
Это удобно когда вы используете много аккаунтов text.ru

```php
$userkey = 'Ваш text.ru userkey';
$text = 'Проверяемый текст, не менее 100 символов';

//Добавляете текст на проверку и сохраняете text_uid для последующего получения результатов
$options = ["exceptdomain"=>"mydomain.ru"]; //Необязательный параметр. Массив дополнительных параметров (см. описание API)
$result = TextRuApi::add($userkey, $text, $options);
$uid = $result["text_uid"];

//Требуется выждать паузу чтобы сервис успел обработать текст.
//Рекомендуется больше минуты.
sleep(15);

//Получаете результат проверки
$jsonvisible = 'detail'; //Необязательный параметр. Укажите "detail" чтобы получить расширенные данные по тексту
$result = TextRuApi::get($userkey, $uid, $jsonvisible);
```


### Получение остатка символов
Получение суммарного остатка символов по всем пакетам
```php
$userkey = 'Ваш text.ru userkey';

$result = TextRuApi::account($userkey);
//или
$app = new \TextRuApi\TextRuApi($userkey);
$result = $app->account();

var_dump($result['size']);
```

## PHPUnit тесты
Запуск из корня компонента
```bash
./vendor/phpunit/phpunit/phpunit --no-coverage
```