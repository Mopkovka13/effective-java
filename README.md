# effective-java
my notes

## Создание и уничтожение объектов

### Применение статических фабричных методов вместо конструкторов

#### Конструктор
```
class Car {
    ...
    public class Car(String brand, String model) {...}
}

Car tayotaCar = new Car("Toyota", "Carolla");
```

#### Статический фабричный метод
```
class Car {
    ...
    public static Car createToyota(String model) {
        return new Car("Toyota", model);
    }
}

Car toyotaCar = Car.createToyota("Corolla");
```
- Можно создавать удобное название, в отличие от конструкторов
- Не обязаны создавать новые объекты при каждом вызове
- Может быть тяжело выявить, что он есть и создать экземпляр

Популярные имена (from, of, valueOf, getInstance, create, getType, newType)

#### Фабрика паттерн (Написан здесь, чтобы понять отличие от фабричного метода)
```
abstract class Car {
    public abstract String getBrand();
}
class Toyota extends Car {
    @Override
    public String getBrand() {
        return "Toyota";
    }
}
class ToyotaFactory extends CarFactory {
    @Override
    public Car createCar() {
        return new Toyota();
    }
}

Car toyotaCar = toyotaFactory.createCar();
```

### Builder вместо конструктора

#### Телескопический конструктор
Конструктор с разным количеством параметров

```
class Person {
    private String name;
    private int age;
    private String address;

    public Person(String name, int age) {
        this(name, age, null, null, null);
    }

    public Person(String name, int age, String address) {
        this(name, age, address, null, null);
    }
}

Person person1 = new Person("Alice", 30);
Person person2 = new Person("Bob", 25, "123 Street");
```

#### javaBeans
Параметры через set
- может быть несогласованность объекта

```
class Person {
    private String name;
    private int age;
    private String address;

    public Person() {}

    // Сеттеры
    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}

Person person2 = new Person();
person2.setName("Bob");
person2.setAge(25);
person2.setAddress("123 Street");
```

#### Builder
- Можно не писать все параметры
```
@Builder
class Person {
    private String name;
    private int age;
    private String address;
}

Person person = Person.builder()
    .address()
    .name()
    .age()
    .build();
```

#### Синглтон с помощью закрытого конструктора или типа перечисления
