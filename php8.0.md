ШПАРГАЛКА ПО ФИЧАМ PHP 8.0
-------------------

В данной памятке выписаны самые основные добавленные фичи в каждой новой версии языка php,
начиная с версии php 8

# PHP 8

### Функции:
1. str_constains(string $haystack, string $needle): bool - Ищет подстроку в строке (RFC https://wiki.php.net/rfc/str_contains)
2. str_starts_with(string $haystack, string $needle): bool - Начинается ли строка с подстроки (RFC https://wiki.php.net/rfc/add_str_starts_with_and_ends_with_functions)
3. str_ends_with  (string $haystack, string $needle): bool - Заканчивается ли строка с подстроки (RFC https://wiki.php.net/rfc/add_str_starts_with_and_ends_with_functions)
4. get_debug_type(mixed $value ): string - Возвращает тип переменной, изменены названия возвращаемых типов, они были приведены в стандартные в большинства языках, вместе boolean - bool, NULL - null и т.п.(RFC https://wiki.php.net/rfc/get_debug_type)
5. fdiv(float $x, float $y): float - Деление чисел с плавающей точкой с учетом стандарта IEEE-754(https://ru.wikipedia.org/wiki/IEEE_754-2008)
6. get_resource_id($resource): int - Приводит ресурс к числу
7. DateTime::createFromInterface()\


### Классы
1. Interface Stringable - Автоматически добавляется к классам, имплиментирующих метод __toString(), можно добавлять объект имплиментирующий метод __toString() в аргументы, как строку (RFC https://wiki.php.net/rfc/stringable)
```php
function printString(string|Stringable): void
```
2. class WeakMap() (RFC https://wiki.php.net/rfc/weak_maps), (дока - https://www.php.net/manual/ru/class.weakmap.php)
3. class PhpToken() (RFC https://wiki.php.net/rfc/token_as_object), (дока - https://www.php.net/manual/ru/class.phptoken.php)
4. Миграция ресурсов в объекты, пример, теперь в php > 8, curl_init возвращает CurlHandle|false, также и с GDImage,Socket, OpenSSL


### Типы данных
1. Новый тип mixed (RFC https://wiki.php.net/rfc/mixed_type_v2)
2. Объединение типов
```php
function test(int|float $nubmer): string|array {}
```
3. static

### Новые конструкции
1. $object::class - теперь Магическая константа class доступна на объектах. (аналог get_class($object)) (RFC https://wiki.php.net/rfc/class_name_literal_on_object)
2. throw можно использовать, как выражение (RFC https://wiki.php.net/rfc/throw_expression)
```php
$foo = $bar ?: throw new Exception('$bar is falsy');
```
3. Выражение match(). Некоторая замена switch() (RFC https://wiki.php.net/rfc/match_expression_v2)
```php
$status = match($request->getMethod()) {
    'POST'        => $this->handlePost($request),
    'GET', 'HEAD' => $this->handleGet($request),
    default       => throw new Exception('Unsupported method'), //также + фича п. №2
};
```
4. Висячая запятая в параметрах и use (RFC https://wiki.php.net/rfc/trailing_comma_in_parameter_list, https://wiki.php.net/rfc/trailing_comma_in_closure_use_list)
5. Свойства в конструкторе (RFC https://wiki.php.net/rfc/constructor_promotion)
```php
// php < 8.0
final class Point
{
    public float $x;
    public float $y;

    public function __construct(float $x = 0.0, float $y = 0.0)
    {
        assert($x >= 0.0);
        assert($y >= 0.0);

        $this->x = $x;
        $this->y = $y;
    }
}

// php >= 8.0

final class Point
{
    public function __construct(
        public float $x = 0.0,
        public float $y = 0.0,
    ) {
        assert($x >= 0.0);
        assert($y >= 0.0);
    }
}
```
6. Атрибуты (RFC https://wiki.php.net/rfc/attributes_v2, https://wiki.php.net/rfc/shorter_attribute_syntax_change)
```php
#[
  ORM\Entity,
  ORM\Table("recipient")
]
final class Recipient
{
    /** @psalm-readonly */
    #[ORM\Id, ORM\Column("uuid")]
    private string $id;

    #[ORM\Column("string", ORM\Column::UNIQUE)]
    private string $email;
}
```
7. Именованные аргументы (RFC https://wiki.php.net/rfc/named_params)
```php
class CustomerData
{
    public function __construct(
        public string $name,
        public string $email,
        public int $age,
    ) {}
}

$data = new CustomerData(
    name: $input['name'],
    email: $input['email'],
    age: $input['age'],
);
```
8. Nullsafe оператор ?-> (RFC https://wiki.php.net/rfc/nullsafe_operator)
```php
function identity(): ?Identity
{
    $token = tokenStorage()->token();

    if ($token === null) {
        return null;
    }

    return $token->identity();
}

function identity(): ?Identity
{
    return tokenStorage()->token()?->identity();
}
```
9. catch без переменной (RFC https://wiki.php.net/rfc/non-capturing_catches)
```php
try {
    return $container->get('logger');
}
- catch (NotFoundExceptionInterface $exception) {
+ catch (NotFoundExceptionInterface) {
    return new NullLogger();
}
```

### Серверные фичи
1. Добавили компилятор JIT (RFC https://wiki.php.net/rfc/jit)
