ШПАРГАЛКА ПО ФИЧАМ PHP 8.1
-------------------

В данной памятке выписаны самые основные(крутые) добавленные фичи в каждой новой версии языка php,
начиная с версии php 8.1

### Функции:
1. `array_is_list` - Проверяет, что array является списком


### Классы
1. Fiber - Файбер позволяет управлять процессом(не Linux процессом а потоком php, к примеру, остановить, сделать что-то свое, и продолжить работу файбера).</br>
Подробнее о реализации: </br>
https://youtu.be/Lkwmawu3Ltc?t=5518
https://php.watch/versions/8.1/fibers#fiber-class
https://www.php.net/manual/ru/language.fibers.php
2. 


### Типы данных
1. Возвращаемый тип `never` - Является подтипом всех типов, не поддерживает return. В классах также можно сужать до never, т.к. это подтип

### Новые конструкции
1. Новое обозначение `о` для восьмеричных чисел, пример, 0о777 === 0777
2. Распаковка ассоциативных массивов оператором "..."
```php
// php < 8.1
$arrayA = ['a' => 1];
$arrayB = ['b' => 2];

$result = array_merge(['a' => 0], $arrayA, $arrayB);

// ['a' => 1, 'b' => 2]

//php >= 8.1
$arrayA = ['a' => 1];
$arrayB = ['b' => 2];

$result = ['a' => 0, ...$arrayA, ...$arrayB];

// ['a' => 1, 'b' => 2]
```
3. Поддержка `final` для констант - В php <=8 в интерфейсах константы были по дефолту final, теперь в php >= 8.1 нет, нужно явно указывать final в интерфейсах.
4. Новый синтаксис для callback-функций
```php
// php < 8.1
$foo = [$this, 'foo'];

$fn = Closure::fromCallable('strlen');

// php >= 8.1
$foo = $this->foo(...);

$fn = strlen(...);
```
5. Поддержка `new` в инициализаторах
```php
// В php <=8 нужно было делать так:

class Service
{
  private Logger $logger;

  public function __construct(
    ?Logger $logger = null,
  ) {
     $this->logger = $logger ?? new NullLogger();
  }
}

// В php >= 8.1, можно сократить до такой записи с помощью new в инициализаторах
class Service
{
    private Logger $logger;
   
    public function __construct(
        Logger $logger = new NullLogger(),
    ) {
        $this->logger = $logger;
    }
}
```
6. Поддержка свойств класса `readonly` конструкции
```
class Service
{
  public readonly string $param;

  public function __construct(string $param) {
     $this->param = $param;
  } 
}

(new Service('value'))->param = 'new value'; // не отработает
```
7. Добавление перечислений `enums` - Подробнее https://www.youtube.com/watch?t=5074&v=Lkwmawu3Ltc&feature=youtu.be
```php
// php < 8.1
class Status
{
    const DRAFT = 'draft';
    const PUBLISHED = 'published';
    const ARCHIVED = 'archived';
}
function acceptStatus(string $status) {...}

// php >= 8.1
enum Status
{
    case Draft;
    case Published;
    case Archived;
}
function acceptStatus(Status $status) {...}
```