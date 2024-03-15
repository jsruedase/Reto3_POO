# Reto3_POO
Tercer Reto POO

Ejercicio Restaurante:
Se crea la clase MenuItem, la cual tiene como subclases los productos del menú, como Burger. Luego, se crea la clase Order la cual posee una lista con MenuItems y ahí hay métodos para calcular el total.

```mermaid
classDiagram
    class MenuItem {
        -name 
        -price 
        -method
    }
    class Burger {
        -type 
        -with_vegetables 
        -doneness 
    }
    class Milkshake {
        -flavor 
        -size 
    }
    class Fries {
        -type 
        -size 
    }
    class Soda {
        -brand: string
        -size: string
    }
    class Pizza {
        -size 
        -type 
    }
    class IceCream {
        -flavor 
        -scoops 
    }
    class HotDog {
        -sauces 
    }
    class ChickenNuggets {
        -pieces 
    }
    class Salad {
        -type 
    }
    class Steak {
        -doneness
    }
    class Order {
        -items_list
        -payment
        +calculate_total()
        +apply_discount(total)
    }

    MenuItem <|-- Burger
    MenuItem <|-- Milkshake
    MenuItem <|-- Fries
    MenuItem <|-- Soda
    MenuItem <|-- Pizza
    MenuItem <|-- IceCream
    MenuItem <|-- HotDog
    MenuItem <|-- ChickenNuggets
    MenuItem <|-- Salad
    MenuItem <|-- Steak
    Order "1" *-- "*" MenuItem : contains

```

"""
There are 10 items on the menu, which have their own prices. However, if certain items are ordered together, 
a discount can be applied to the combo. The combos are:
    Personal: CheeseBurger + Fries + Soda             -10%
    Personal: Pizza + Milkshake                       -20%
    Familiar: ChickenNuggets + HotDog + Steak + Salad -30%

Aditionaly, the user can modify the size of some items, which will increase the base price. There are two options:
medium or large. The large size increases the base price by 25%.

Finally, there are three methods of payment: cash, card or nequi. The cash method has a 5% discount, the card increases
the total price by 5% and the nequi method has no discount or increase.
"""

class MenuItem:
    def __init__(self, name, price, method):
        self.name = name
        self.price = price
        self.method = method
        
class Burger(MenuItem):
    def __init__(self, name, price, type, with_vegetables, doneness, method) -> None:
        super().__init__(name, price, method)
        self.type = type
        self.with_vegetables = with_vegetables
        self.doneness = doneness
        
class Milkshake(MenuItem):
    def __init__(self, name, price, flavor, size, method) -> None:
        super().__init__(name, price, method)
        self.flavor = flavor
        self.size = size

class Fries(MenuItem):
    def __init__(self, name, price, type, size, method) -> None:
        super().__init__(name, price, method)
        self.size = size
        self.type = type
        
class Soda(MenuItem):
    def __init__(self, name, price, brand, size, method) -> None:
        super().__init__(name, price, method)
        self.brand = brand
        self.size = size

class Pizza(MenuItem):
    def __init__(self, name, price, size, type, method) -> None:
        super().__init__(name, price, method)
        self.size = size
        self.type = type

class IceCream(MenuItem):
    def __init__(self, name, price, flavor, scoops, method) -> None:
        super().__init__(name, price, method)
        self.flavor = flavor
        self.scoops = scoops
        
class HotDog(MenuItem):
    def __init__(self, name, price, sauces, method) -> None:
        super().__init__(name, price, method)
        self.sauces = sauces

class ChickenNuggets(MenuItem):
    def __init__(self, name, price, pieces, method) -> None:
        super().__init__(name, price, method)
        self.pieces = pieces

class Salad(MenuItem):
    def __init__(self, name, price, type, method) -> None:
        super().__init__(name, price, method)
        self.type = type
        
class Steak(MenuItem):
    def __init__(self, name, price, doneness, method) -> None:
        super().__init__(name, price, method)
        self.doneness = doneness
        
class Order():
    def __init__(self, items_list, payment) -> None:
        self.items_list = items_list
        self.payment = payment
        
    def calculate_total(self):  
        total = 0
        for item in self.items_list:
            try:                                    #Some items don't have size atribute
                if item.size == "2":
                    total += item.price * 1.25
                else:
                    total += item.price
            except:
                total += item.price
        return total

    def apply_discount(self, total):
        personal_combo = set(["CheeseBurger", "Fries", "Soda"])
        personal_combo2 = set(["Pizza", "Milkshake"])
        familiar_combo = set(["ChickenNuggets", "HotDog", "Steak", "Salad"])
        items = []
        for item in self.items_list:
            items.append(item.name)
        items = set(items)
        
        #Combos discounts
        if items == personal_combo:
            total = total * 0.9
            print("Personal combo discount applied")
        elif items == personal_combo2:
            total = total * 0.8
            print("Personal combo 2 discount applied")
        elif items == familiar_combo:
            total = total * 0.7
            print("Familiar combo discount applied")
        else:
            print("No combo discount applied")
        
        #Payments
        if self.payment == "cash":
            total = total * 0.95
            print("Cash payment applied")
        elif self.payment == "card":
            total = total * 1.05    
            print("Card payment applied")
        else:
            print("Nequi payment applied")
        return total

if __name__ == "__main__":
    print("Welcome, please select the items you want to order:")
    print("1. CheeseBurger")
    print("2. Fries")
    print("3. Soda")
    print("4. Pizza")
    print("5. Milkshake")
    print("6. IceCream")
    print("7. HotDog")
    print("8. ChickenNuggets")
    print("9. Salad")
    print("10. Steak")
    print("0. Finish order")   
    payment = input("Please select the payment method: (cash, card, nequi)\n")
    ordering = True
    order_list = []
    
    while ordering:
        orden = input("Select the item you want to order or type 0 to finish the order: ")
        if orden == "1":
            type = input("Select the type of burger: \n1. Beef + 0$\n2. Chicken +2000$ \n3. Veggie +3000$\n")
            with_vegetables = input("Do you want it with vegetables? (yes/no)\n")
            doneness = input("How do you want it cooked? \n1. Rare\n2. Medium \n3. Well done \n")
            if type == "2":
                order_list.append(Burger("CheeseBurger", 17000, type, with_vegetables, doneness, payment))
            elif type == "3":
                order_list.append(Burger("CheeseBurger", 18000, type, with_vegetables, doneness, payment))
            else:
                order_list.append(Burger("CheeseBurger", 15000, type, with_vegetables, doneness, payment))
            
            
        elif orden == "2":
            type = input("Select the type of fries: \n1. Regular +0$\n2. Curly +1000$\n")
            size = input("Select the size: \n1. Medium +0$\n2. Large +1750$\n")
            if type == "2":
                order_list.append(Fries("Fries", 8000, type, size, payment))
            else:
                order_list.append(Fries("Fries", 7000, type, size, payment))
            
        elif orden == "3":
            brand = input("Select the brand of the soda: \n1. CocaCola \n2. Pepsi \n")
            size = input("Select the size: \n1. Medium +0$\n2. Large +1250$\n")
            order_list.append(Soda("Soda", 5000, brand, size, payment)) 
            
        elif orden == "4":
            size = input("Select the size of the pizza: \n1. Medium +0$\n2. Large +2500$\n")
            type = input("Select the type of pizza: \n1. Pepperoni +0$\n2. Hawaiian +0$\n")
            order_list.append(Pizza("Pizza", 10000, size, type, payment))
        
        elif orden == "5":
            flavor = input("Select the flavor of the milkshake: \n1. Vanilla \n2. Chocolate \n")
            size = input("Select the size: \n1. Medium +0$\n2. Large +2500$\n")
            order_list.append(Milkshake("Milkshake", 10000, flavor, size, payment))
            
        elif orden == "6":
            flavor = input("Select the flavor of the ice cream: \n1. Vanilla +0$\n2. Chocolate +0$\n")
            scoops = input("Select the number of scoops: \n1. One +0$\n2. Two +2000$\n")
            if scoops == "2":
                order_list.append(IceCream("IceCream", 7000, flavor, scoops, payment))
            else:
                order_list.append(IceCream("IceCream", 5000, flavor, scoops, payment))
            
        
        elif orden == "7":
            sauces = input("Select the sauces: \n1. Ketchup \n2. Mustard \n")
            order_list.append(HotDog("HotDog", 8000, sauces, payment))
        
        elif orden == "8":
            pieces = input("Select the number of pieces: \n1. Six \n2. Twelve +10000$\n")
            if pieces == "2":
                order_list.append(ChickenNuggets("ChickenNuggets", 22000, pieces, payment))
            else:
                order_list.append(ChickenNuggets("ChickenNuggets", 12000, pieces, payment))

        elif orden == "9":
            type = input("Select the type of salad: \n1. Caesar \n2. Garden \n")
            order_list.append(Salad("Salad", 5000, type, payment))
        
        elif orden == "10":
            doneness = input("How do you want the steak cooked? \n1. Rare \n2. Medium \n3. Well done \n")
            order_list.append(Steak("Steak", 30000, doneness, payment))
            
        elif orden == "0":
            print("Order finished")
            ordering = False
        else:
            print("Invalid, please select a correct option")
            
    order = Order(order_list, payment)
    print(f"Total price: {order.calculate_total()}")
    print(f"Total price according to payment method and discount combos: {order.apply_discount(order.calculate_total())}")


