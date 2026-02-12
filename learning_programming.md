**Learning Programming**
===

# Interpreted vs Compiled

executed without compiling first (runtime reads actual code, not bytecode)

![](images/learning_programming_interpreted-vs-compiled.png)

Source: [Blurred Lines: Is Ruby an interpreted language and what does that even mean? - medium.com](https://medium.com/@astermanuelg/blurred-lines-is-ruby-an-interpreted-language-2d3d6bca3d37)

# Type System

![](images/learning_programming_type-system.png)

Source: [What is the difference between statically typed and dynamically typed languages? - stackoverflow.com](https://stackoverflow.com/questions/1517582/what-is-the-difference-between-statically-typed-and-dynamically-typed-languages)

## Dynamic vs Static

- Dynamic
  - e.g. JavaScript
  - variable types determinded at runtime, instead of at compile time (run the program, then check the types)
  - can be easily identified by generic variables like `let x = 5;`
- Static
  - e.g. Java, C++
  - check the types, then run the program
  - specific variables like `int x = 5;`

## Weak vs Strong

- Weak
  - types of variables are not enforced very strongly
  - Example: `"The answer is " + x`, output `The answer is 5`
- Strong
  - e.g. Python, Ruby
  - Example: `"The answer is " + x`, output `error`

# Paradigms

## Functional Programming

## Object-Oriented Programming (OOP)

Source: [A Beginner’s Guide to Object-Oriented Programming (OOP) in Python - 200oksolutions.com](https://200oksolutions.com/blog/object-oriented-programming-in-python/)

Object-Oriented Programming (OOP) is a paradigm where everything is treated as an object that combines data (attributes) and behavior (methods). The primary goal of OOP is to simplify software development and make code more modular, flexible, and scalable.

>OOP encourages more readable and maintainable code by organizing it into reusable components.

### Class

A blueprint for creating objects. It defines attributes (variables) and methods (functions).

``` python
class Car:
    """Initialize the car class. (Constructor)"""
    def __init__(self, name, model):
        self.name = name
        self.model = model

    def __str__(self):
        """Return a string representation of the car."""
        return f"{self.name} {self.model}"

    def start_engine(self):
        """Start the car engine."""
        print("Engine started")


toyota = Car("Toyota", "Corolla")
print(toyota)
```

In this example:

- `Car` is the class, and it has attributes like `name` and `model`, which you set in the constructor.
- The `__str__` method provides a custom string representation for the object, so when you print an instance like `toyota`, it outputs `Toyota Corolla`.

### Object

An instance of a class containing data and behaviors.

``` python
toyota = Car("Honda", "Odyssey")
toyota.start_engine()
```

Here, `toyota` is an object of the `Car` class with unique values for `name` (“Honda”) and `model` (“Odyssey”).

### Inheritance

Inheritance is the mechanism of creating a new class from an existing class. The new class, called the **subclass**, inherits attributes and methods of the existing class, known as the **superclass**. This helps in reusing and extending existing code without duplication.

``` python
class Car:
    """Initialize the car class. (Constructor)"""

    def __init__(self, name, model):
        self.name = name
        self.model = model

    def __str__(self):
        """Return a string representation of the car."""
        return f"{self.name} {self.model}"

    def start_engine(self):
        """Start the car engine."""
        print("Engine started")


class ElectricCar(Car):
    """Initialize the electric car class. (Constructor)"""

    def __init__(self, name, model, battery_size=75):
        """Initialize attributes of the parent class."""
        Car.__init__(self, name, model)
        self.battery_size = battery_size

    def describe_battery(self):
        """Print a statement describing the battery size."""
        print(f"This car has a {self.battery_size}-kWh battery.")


toyota =
Car("Toyota", "Corolla")
evcar = ElectricCar("Tesla", "Model S")
evcar.describe_battery()

evcar.start_engine()
```

In this example, `Car` is the base class (parent class) and `ElectricCar` is inherited from the `Car` class. We are creating an electric car as a specialized version of a car with additional functionality specific to cars, like describing battery size.

Methods:

- `describe_battery`: This method prints the size of the battery, which is specific to electric cars.
- `start_engine`: This method is inherited from the parent class. So, `ElectricCar` has access to both attributes and methods of the `Car` class.

### Polymorphism

Polymorphism allows methods to be used interchangeably between different classes, making code more flexible. Polymorphism is commonly achieved through **method overriding**, where a subclass provides a specific implementation of a method that already exists in its superclass.

``` python
class Car:
  def drive(self):
      return "Driving a car."

class Bike:
    def drive(self):
        return "Riding a bike."

honda = Car()
ducati = Bike()

for vehicle in [honda, ducati]:
    print(vehicle.drive())
```

Here, both `Car` and `Bike` have a drive method, but each behaves differently. Polymorphism allows us to call `drive()` on each vehicle without needing to know its specific class.

### Encapsulation

Encapsulation is the practice of hiding internal details of an object and only exposing what’s necessary. This is often achieved by making attributes **private** (using an underscore prefix) and providing **getter** and **setter** methods to access or modify these attributes safely.

``` python
# "__" double underscore represents private attribute.

class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # private attribute

    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            return f"Deposited {amount}"

    def get_balance(self):
        return f"Balance: {self.__balance}"

account = BankAccount(100)
print(account.get_balance())       # Output: Balance: 100
print(account.__balance)           # Raises an AttributeError
```

In this example:

- The `__balance` attribute is private and can’t be accessed directly outside of the class.
- It can only be accessed or modified through `get_balance` and `deposit` methods, ensuring the balance is managed correctly.

### Abstraction

Abstraction is the concept of simplifying complex systems by modeling classes with only essential attributes and methods. It hides unnecessary details and focuses on functionality, making code cleaner and easier to understand.

In Python, we often use **abstract classes** to create a common interface for subclasses.

``` python
import math

class Circle:
    def __init__(self, radius):
        self.__radius = radius  # Private attribute

    def area(self):
        """Calculate the area of the circle."""
        return math.pi * (self.__radius ** 2)

    def circumference(self):
        """Calculate the circumference of the circle."""
        return 2 * math.pi * self.__radius

# Create an instance of Circle
circle = Circle(5)
print(f"Area: {circle.area():.2f}")          # Output: Area: 78.54
print(f"Circumference: {circle.circumference():.2f}")  # Output: Circumference: 31.42
# Trying to access the private attribute will raise an AttributeError
# print(circle.__radius)  # This will raise an AttributeError
```

In this example:

- The `__radius` attribute is private, so it can’t be accessed directly outside the `Circle` class.
- The area and circumference can only be calculated through the `area()` and `circumference()` methods, ensuring the radius is used properly without exposing its details.


# Mutable vs Immutable

Mutable can be changed. Immutable cannot.

# Single Threaded vs Multithreaded