# SOLID

### Índice

1. [x] [S — Single Responsiblity Principle (Princípio da responsabilidade única)](https://github.com/deviobr/code-patterns/blob/main/SOLID.md#s--single-responsiblity-principle-princípio-da-responsabilidade-única)
2. [x] [O — Open-Closed Principle (Princípio Aberto-Fechado)](https://github.com/deviobr/code-patterns/blob/main/SOLID.md#o--open-closed-principle-princípio-aberto-fechado)
3. [x] [L — Liskov Substitution Principle (Princípio da substituição de Liskov)](https://github.com/deviobr/code-patterns/blob/main/SOLID.md#l---liskov-substitution-principle-princípio-da-substituição-de-liskov)
4. [x] [I — Interface Segregation Principle (Princípio da Segregação da Interface)](https://github.com/deviobr/code-patterns/blob/main/SOLID.md#i--interface-segregation-principle-princípio-da-segregação-da-interface)
5. [x] [D — Dependency Inversion Principle (Princípio da inversão da dependência)](https://github.com/deviobr/code-patterns/blob/main/SOLID.md#d--dependency-inversion-principle-princípio-da-inversão-da-dependência)

## S — Single Responsibility Principle (Princípio da responsabilidade única)
Classes devem ter somente uma responsabilidade. Quando se inicia em OOP, é comum vermos classes como um "repositório de funções" e, com isso, acabarmos por criar uma *GodClass*.

Isso acaba por gerar problemas quando diversos lugares do código dependem dessa classe e, por vezes, uma modificação pode gerar bugs em lugares inesperados.

### Exemplo de classe que fere o princípio
```php
class UserService
{
    public function create(array $data): array
    {
        // ...
    }

    public function read(int $userId): array
    {
        // ...
    }

    public function update(array $data, int $userId): array
    {
        // ...
    }

    public function delete(int $userId): array
    {
        // ...
    }
    
    public function renderUser(array $user): string
    {
        echo $user['name'];
    }

    public function getFirstName(array $user): string
    {
        // ...
    }

    public function calcUserAge(array $user): int
    {
        // ...
    }
}
```

Veja que no exemplo, essa classe pode ser usada por um controller por exemplo,
para executar ações de um CRUD, por uma view ou por qualquer outra classe do projeto.

### Exemplo refatorado
```php
class User
{
    public function getFirstName(): string
    {
        // ...
    }

    public function calcUserAge(): int
    {
        // ...
    }
}

class UserRepository
{
    public function create(array $data): User
    {
        // ...
    }

    public function read(int $userId): User
    {
        // ...
    }

    public function update(array $data, User $user): User
    {
        // ...
    }

    public function delete(User $user): bool
    {
        // ...
    }
}

class UserViewer
{
    public function renderUser(array $user): string
    {
        echo $user['name'];
    }
}
```

## O — Open-Closed Principle (Princípio Aberto-Fechado)
Um dos princípios que mais considero importante. Basicamente uma classe deve ser fechada para modificações, e aberta para extensões. Isso ocorre por ser muito difícil, ao se modificar algo que já funciona bem, o fazer sem criar um bug.

### Exemplo de classe que fere o princípio
Um exemplo muito utilizado é o de cálculo de área de formas geométricas, onde cada forma tem sua particularidade.
```php
class Square
{
    public float $length;
}

class Circle
{
    public float $radius;
}

class AreaCalculator
{
    public function __construct(
        private array $shapes
    ) {}

    public function getAreaOfShapes(): float
    {
        return array_reduce($this->shapes, function ($area, $shape) {
            if ($shape instanceof Square) {
                $squareArea = pow($shape->length, 2);

                return $area + $squareArea;
            }

            if ($shape instanceof Circle) {
                $circleArea = pi() * pow($shape->radius, 2);

                return $area + $circleArea;
            }
        }, 0);
    }
}
```

### Exemplo refatorado
Veja que, se decidirmos inserir um novo shape, teríamos que modificar a classe para suportá-lo. Mas, se dependermos de um interface relacionada às formas, não precisamos fazer isso:

```php
interface Shape
{
    public function getArea(): float;
}

class Square implements Shape
{
    public float $length;

    public function getArea(): float
    {
        return pow($this->length, 2);
    }
}

class Circle implements Shape
{
    public float $radius;

    public function getArea(): float
    {
        return pi() * pow($this->radius, 2)
    }
}

class AreaCalculator
{
    /**
     * @param Shape[] $shapes
     * 
     * @return void
     */
    public function __construct(
        private array $shapes
    ) {}

    public function getAreaOfShapes(): float
    {
        return array_reduce($this->shapes, function ($area, $shape) {
            return $area + $shape->getArea();
        }, 0);
    }
}
```

## L - Liskov Substitution Principle (Princípio da substituição de Liskov)

O princípio diz que:

> Seja S um subtipo de T. S deve pode realizar as mesmas ações de T sem causar problemas

Ou seja, apenas utilize herança em objetos que **SÃO** a classe pai.

### Exemplo de classe que fere o princípio

A classe ```Human``` não deve extender a classe ```Dog```, mesmo ambas contendo, por exemplo, o método ```walk```, pois um humano não consegue balançar a calda, que é outro método da classe ```Dog```. Prefira herança de abstrações ou traits.

```php
class Dog
{
    public function wagTail()
    {
        echo 'Balançando a calda...';
    }
    
    public function walk()
    {
        echo 'Andando...';
    }
}

class Human extends Dog
{
    public function wagTail()
    {
        throw new LogicException('Um humano não possui calda');
    }
    
    public function talk()
    {
        echo 'Falando...';
    }
}
```

### Exemplo refatorado

```php
abstract class Mamifer
{
    public function walk()
    {
        echo 'Andando...';
    }
}

class Dog extends Mamifer
{
    public function wagTail()
    {
        echo 'Balançando a calda...';
    }
}

class Human extends Mamifer
{
    public function talk()
    {
        echo 'Falando...';
    }
}
```

## I — Interface Segregation Principle (Princípio da Segregação da Interface)

O princípio diz que

> Uma classe nunca deve depender de uma interface que ela não utiliza e interfaces não devem depender de detalhes. Detalhes devem depender de interfaces.

Quando uma classe depende de algo que ela não utiliza, é inútil e pode causar bugs inesperados.

### Exemplo de classe que fere o princípio

```php
interface Teacher
{
    public function teachEnglish();
    
    public function teachHistory();
}

class EnglishTeacher implements Teacher
{
    public function teachEnglish()
    {
        echo 'Ensinando inglês...';
    }
    
    public function teachHistory()
    {
        throw new DomainException('Não sei ensinar história');
    }
}

class EnglishStudent
{
    public function __construct(
        private Teacher $teacher
    ) {}
}
```

Não é necessário para um aluno de inglês que seu professor seja também um professor de história ou não, pois isso não interessa a ele.

### Exemplo refatorado

```php
interface EnglishTeacher
{
    public function teachEnglish();
}

interface HistoryTeacher
{
    public function teachHistory();
}

class EnglishTeacher implements EnglishTeacher
{
    public function teachEnglish()
    {
        echo 'Ensinando inglês...';
    }
}

class EnglishStudent
{
    public function __construct(
        private EnglishTeacher $englishTeacher
    ) {}
}
```

## D — Dependency Inversion Principle (Princípio da inversão da dependência)

O princípio diz que:

> Tanto classes de baixo nível quanto de alto devem depender de abstrações, e não de implementações.

Ou seja, uma classe não deve saber como algo é feito, e sim se pode ser feito.

### Exemplo de classe que fere o princípio

Um exemplo comum é um serviço depender de uma classe para lidar com banco de dados.

```php
class MySQLManager
{
    public function insert(string $table, array $data)
    {
         // ...
    }
    
    public function delete(string $table, int $id)
    {
        // ...
    }
    
    // ...
}

class UserService
{
    public function __construct(
        private MySQLManager $dbManager
    ) {}
}
```

O problema é que não é importante para a ```UserService``` qual tipo de banco de dados está sendo utilizado para fazer inserções, por exemplo. Se a aplicação migrasse de MySQL para PostgreSQL, por exemplo, o service teria que ser alterado.

### Exemplo refatorado

```php
interface DatabaseManager
{
    /**
     * @param string $context - Table para bancos de dados relacionais e collections, por exemplo, para não relacionais
     * @param array  $data
    */
    public function insert(string $context, array $data);
    
    /**
     * @param string $context - Table para bancos de dados relacionais e collections, por exemplo, para não relacionais
     * @param mixed  $id
    */
    public function delete(string $context, $id);
}

class MySQLManager implements DatabaseManager
{
    public function insert(string $context, array $data)
    {
         // ...
    }
    
    public function delete(string $context, $id)
    {
        // ...
    }
}


class PostgreSQLManager implements DatabaseManager
{
    public function insert(string $context, array $data)
    {
         // ...
    }
    
    public function delete(string $context, $id)
    {
        // ...
    }
}

class UserService
{
    public function __construct(
        private DatabaseManager $dbManager
    ) {}
}
```

Veja que agora não importa qual tipo de banco de dados será utilizado porque a classe ```UserService``` depende de uma abstração de um gerenciador de banco de dados, e não de uma implementação.

## Recomendações de leitura

1. [The SOLID Principles of Object-Oriented Programming Explained in Plain English](https://www.freecodecamp.org/news/solid-principles-explained-in-plain-english/)
2. [DIP in the Wild](https://martinfowler.com/articles/dipInTheWild.html)
