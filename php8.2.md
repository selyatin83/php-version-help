ШПАРГАЛКА ПО ФИЧАМ PHP 8.2
-------------------

В данной памятке выписаны самые основные добавленные фичи в каждой новой версии языка php,
начиная с версии php 8.2

### Новые конструкции
1. Readonly классы
```php
readonly class BlogData
{
    public string $title;

    public Status $status;

    public function __construct(string $title, Status $status)
    {
        $this->title = $title;
        $this->status = $status;
    }
}
```
2. Типы в виде дизъюнктивной нормальной формы (ДНФ).
Теперь вместо того, чтобы указывать тип mixed для аргументов в методах, можно просто перечислять необходимые типы через амперсанд.<br>

php < 8.2
```php
class Foo {
    public function bar(mixed $entity) {
        if ((($entity instanceof A) && ($entity instanceof B)) || ($entity === null)) {
            return $entity;
        }

        throw new Exception('Invalid entity');
    }
}
```
php 8.2
```php
class Foo {
    public function bar((A&B)|null $entity) {
        return $entity;
    }
}
```
3. Самостоятельные типы null, false и true
4. Константы в трейтах. Особенность фичи — нельзя получить доступ к константе через имя трейта, но можно через класс, который использует этот трейт.
5. Динамические свойства стали устаревшими (\stdClass это не касается)
6. #[\SensitiveParameter] - Возможность указывать параметры метода, как секретные и их значения надо скрывать