## Три принципа ООП в Python 

### 1. Инкапсуляция — сокрытие состояния

**Суть:** объект скрывает свои внутренние данные и предоставляет **контролируемый интерфейс** (методы / свойства). Это защищает состояние от некорректных изменений и уменьшает связанность.

**В Python:**  
- `_атрибут` – защищённый (по соглашению, но доступ есть).  
- `__атрибут` – приватный (name mangling: `_Класс__атрибут`).  
- `@property` – контроль доступа без потери синтаксиса поля.

**Пример:**

```python
class Animal:
    def __init__(self, name, age):
        self.name = name
        self.__age = age          # приватный атрибут

    @property
    def age(self):
        return self.__age

    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Возраст не может быть отрицательным")
        self.__age = value

a = Animal("Лев", 5)
print(a.age)    # 5 – через геттер
a.age = 7       # работает сеттер с проверкой
# a.__age       # AttributeError – доступ закрыт
```

---

### 2. Наследование — переиспользование и иерархии

**Суть:** дочерний класс получает всё от родителя, может **добавлять** и **переопределять** поведение. Python поддерживает множественное наследование с разрешением конфликтов через MRO (C3-линеаризацию).

**Ключевые механизмы:**  
- `super()` – вызов следующего класса в иерархии MRO, нужен для кооперативного наследования.  
- `@abstractmethod` (из `abc`) – задаёт контракт для наследников.

**Пример:**

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    def __init__(self, name):
        self.name = name

    @abstractmethod
    def speak(self):
        pass

class Cat(Animal):
    def speak(self):
        return "Мяу"
    
    def purr(self):
        return "Мур-мур"

c = Cat("Мурка")
print(c.name)      # унаследовано
print(c.speak())   # переопределено
print(c.purr())    # добавлено в Cat
```

---

### 3. Полиморфизм — единый интерфейс, разная реализация

**Суть:** один и тот же вызов метода даёт разный результат в зависимости от actual типа объекта. Две формы:  
- **Полиморфизм подтипов** (через наследование и переопределение).  
- **Утиная типизация** (важен не тип, а наличие метода) – квинтэссенция Python.

**Пример с утиной типизацией:**

```python
class Dog:
    def speak(self):
        return "Гав"

class Cat:
    def speak(self):
        return "Мяу"

class Duck:
    def speak(self):
        return "Кря"

def animal_sound(animal):
    # Не важно, кто пришёл — лишь бы был .speak()
    print(animal.speak())

animal_sound(Dog())   # Гав
animal_sound(Cat())   # Мяу
animal_sound(Duck())  # Кря
```

**То же через наследование (формальный полиморфизм):**

```python
def make_sound(animal: Animal):
    print(animal.speak())

make_sound(Cat("Барсик"))   # Мяу
```

---

