# Poo-Reto-8
Se retoma el codigo creado en el Reto 3, donde se creaba la clase `Menu`, y se crea una nueva clase para realizar iteraciones con los items añadidos al menú.

```Python
class MenuItem:
    def __init__(self, name: str, price: float):
        self.name = name
        self.price = price

    def calculate_total_price(self, quantity: int = 1) -> float:
        return self.price * quantity


class Beverage(MenuItem):
    def __init__(self, name: str, price: float, is_alcoholic: bool = False):
        super().__init__(name, price)
        self.is_alcoholic = is_alcoholic


class Appetizer(MenuItem):
    def __init__(self, name: str, price: float, is_shared: bool = True):
        super().__init__(name, price)
        self.is_shared = is_shared


class MainCourse(MenuItem):
    def __init__(self, name: str, price: float, is_vegetarian: bool = False):
        super().__init__(name, price)
        self.is_vegetarian = is_vegetarian


class OrderIterator:
    def __init__(self, order):
        self._order = order
        self._index = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self._index < len(self._order.items):
            item, quantity = self._order.items[self._index]
            self._index += 1
            return item, quantity
        else: 
            raise StopIteration 


class Order:
    def __init__(self):
        self.items = []

    def add_item(self, item: MenuItem, quantity: int = 1):
        self.items.append((item, quantity))

    def calculate_total(self) -> float:
        total = sum(item.calculate_total_price(quantity) for item, quantity in self.items)
        return total

    def apply_discount(self, percentage: float) -> float:
        total = self.calculate_total()
        discount = total * (percentage / 100)
        return total - discount

    def print_order(self):
        print("Detalles del pedido:")
        for item, quantity in self.items:
            print(f"{quantity}x {item.name} - ${item.price:.2f} cada uno")
        print(f"Total: ${self.calculate_total():.2f}")
    
    def __iter__(self):
        return  OrderIterator(self)


order = Order()
order.add_item(Beverage("Cerveza", 2.50, is_alcoholic = True), quantity = 2)
order.add_item(Appetizer("Nachos", 6.00, is_shared = True), quantity = 1)
order.add_item(MainCourse("Pizza Napolitana", 12.00, is_vegetarian = True), quantity=1)

order.print_order()
print(f"Total con 10% de descuento: ${order.apply_discount(10):.2f}")

print("\nIterando los elementos del pedido:")
for item, quantity in order:
    print(f"{quantity}x {item.name} - ${item.price:.2f} cada uno")

```
