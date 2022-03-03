# SOLID

### TODO

1. [x] [S — Single Responsiblity Principle (Princípio da responsabilidade única)](https://github.com/gabrieljmj/devio-dev-doc/blob/main/SOLID.md#s--single-responsiblity-principle-princípio-da-responsabilidade-única)
2. [x] [O — Open-Closed Principle (Princípio Aberto-Fechado)](https://github.com/gabrieljmj/devio-dev-doc/blob/main/SOLID.md#o--open-closed-principle-princípio-aberto-fechado)
3. [ ] L — Liskov Substitution Principle (Princípio da substituição de Liskov)
4. [ ] I — Interface Segregation Principle (Princípio da Segregação da Interface)
5. [ ] D — Dependency Inversion Principle (Princípio da inversão da dependência)

## S — Single Responsiblity Principle (Princípio da responsabilidade única)
Classes devem ter somente uma responsabilidade. Quando se inicia em OOP, é comum
vermos classes como um "repositório de funções" e, com isso, acabarmos por criar
uma *GodClass*.

Isso acaba por gerar problemas quando diversos lugares do código dependem dessa
classe e, por vezes, uma modificação pode bugs em lugares inesperados.

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
para executar ações de um CRUD, ou por uma view, ou por qualquer outra classe
do projeto.

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
Um dos princípios que mais considero importante. Basicamente um classe deve ser
fechada para modificações, e aberta para extensões. Isso ocorre por ser muito
fácil, ao se modificar algo que já funciona bem, o fazer sem criar um bug.

### Exemplo de classe que fere o princípio
Um exemplo muito utilizado é o de cálculo de área de formas geométricas, onde
cada forma tem sua particularidade.
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
Veja que, se decidirmos inserir um novo shape, teriamos que modificar a classe
para suportá-lo. Mas, se dependermos de um interface relacionada às formas, não
precisamos fazer isso:

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

## Recomendações de leitura

1. [The SOLID Principles of Object-Oriented Programming Explained in Plain English](https://www.freecodecamp.org/news/solid-principles-explained-in-plain-english/)
2. [DIP in the Wild](https://martinfowler.com/articles/dipInTheWild.html)
3. ...
