### Design Patterns Applied in the Pizza Restaurant System

#### 1. Singleton Pattern

**Description**: The Singleton pattern ensures that a class has only one instance and provides a global point of access to it. In this system, the `InventoryManager` class uses the Singleton pattern to manage the inventory consistently across the application.

**Application in System**:
- The `InventoryManager` ensures that there is a single, shared inventory of ingredients for all pizzas and toppings.

**Before Applying the Pattern**:
- Without the Singleton, multiple instances of `InventoryManager` could have been created, leading to inconsistent inventory updates.

**Advantages**:
- Only one store per data type for all data
- Consistent ingredient availability checks.

**Code Snippet**:
```python
class InventoryManager:
    _inventory = {
        "Margherita": 10,
        "Pepperoni": 10,
        "Cheese": 15,
        "Olives": 10,
        "Mushrooms": 12,
    }
    def check_and_decrement(self, item: str) -> bool:
        if self._inventory.get(item, 0) > 0:
            self._inventory[item] -= 1
            return True
        return False

    def get_inventory(self):
        return self._inventory
```

#### 2. Factory Pattern

**Description**: The Factory pattern encapsulates the creation logic of objects. In this system, the `PizzaFactory` creates instances of `Margherita` or `Pepperoni` based on user input and checks availability.

**Application in System**:
- Simplifies the pizza creation process by abstracting the logic into a single class.
- Ensures consistency in object creation and reduces code duplication.

**Before Applying the Pattern**:
- Pizza creation logic would have been scattered across the code, making it harder to maintain and extend.

**Advantages**:
- Centralized pizza creation logic.
- Easy to add new pizza types in the future.

**Code Snippet**:
```python
class PizzaFactory:
    @staticmethod
    def create_pizza(pizza_type: str) -> Pizza:
        inventory = InventoryManager()
        if pizza_type == "Margherita" and inventory.check_and_decrement("Margherita"):
            return Margherita()
        elif pizza_type == "Pepperoni" and inventory.check_and_decrement("Pepperoni"):
            return Pepperoni()
        else:
            raise ValueError("Pizza type unavailable or out of stock!")
```

#### 3. Decorator Pattern

**Description**: The Decorator pattern allows behavior to be dynamically added to an object at runtime. In this system, it is used to add toppings (e.g., Cheese, Olives, Mushrooms) to pizzas.

**Application in System**:
- Each topping is a decorator that wraps the pizza object and updates its description and cost.
- Enables dynamic addition of toppings without modifying the original pizza classes.

**Before Applying the Pattern**:
- Adding toppings would require modifying the base pizza classes, violating the Open/Closed Principle.

**Advantages**:
- Flexible and extensible design.
- New toppings can be added without changing existing classes.

**Code Snippet**:
```python
class Cheese(ToppingDecorator):
    def get_description(self) -> str:
        return self._pizza.get_description() + ", Cheese"

    def get_cost(self) -> float:
        return self._pizza.get_cost() + 1.0
```

### Overengineering
**Definition**: Overengineering occurs when a system is made more complex than necessary, often by adding features or abstractions that are not needed.

**Example**:
In the pizza system, creating a separate strategy class for each topping could be considered overengineering because toppings are simple and do not require complex logic.

**Code Snippet of Overengineering**:
```python
class Cheese(ToppingDecorator):
    def get_description(self) -> str:
        return self._pizza.get_description() + ", Cheese"

    def get_cost(self) -> float:
        return self._pizza.get_cost() + 1.0

class Olives(ToppingDecorator):
    def get_description(self) -> str:
        return self._pizza.get_description() + ", Olives"

    def get_cost(self) -> float:
        return self._pizza.get_cost() + 0.5

class Mushrooms(ToppingDecorator):
    def get_description(self) -> str:
        return self._pizza.get_description() + ", Mushrooms"

    def get_cost(self) -> float:
        return self._pizza.get_cost() + 0.7

```
**Why it's Overengineering**:
- Adds unnecessary complexity.
- The Decorator pattern already handles toppings effectively.

---

