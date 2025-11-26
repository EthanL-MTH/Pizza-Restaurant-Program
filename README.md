# Pizza-Restaurant-Program
For this program the task was to create a backend software for employee's to take the customer details, the amount of customers, the quantity, size and toppings of the pizzas ordered and if the customer requires delivery, calculating the cost and applying any discount available. After which an itemized bill is produced.

#Standard delivery charge
DELIVERY_COST = 2.50

#The base cost of the different sizes of pizzas
S_PIZZA = 3.25
M_PIZZA = 5.50
L_PIZZA = 7.15

#Topping cost relative to number of toppings
ONE_TOP = 0.75
TWO_TOP = 1.35
THREE_TOP = 2.00
FOUR_PLUS_TOP = 2.50

#Discount price for orders over £20
DISCOUNT = 0.10

#The function used to find the size of the pizza, looping until its a valid size
def sizePizzaFind(sizePizza):
    while True:
        if sizePizza.lower() == "small":
            pricePizza = S_PIZZA
            break
        elif sizePizza.lower() == "medium":
            pricePizza = M_PIZZA
            break
        elif sizePizza.lower() == "large":
            pricePizza = L_PIZZA
            break
        else:
            print("Invalid size of pizza")
            sizePizza = str(input(f"Please re-enter the size of the pizza (Small, Medium or Large): "))
    return pricePizza

#The function which calculates the topping cost for the number of toppings by comparing toppingNum to different values
def toppingNumber(toppingNum):
    if toppingNum == 1:
        toppingCost = ONE_TOP
    elif toppingNum == 2:
        toppingCost = TWO_TOP
    elif toppingNum == 3:
        toppingCost = THREE_TOP
    else:
        toppingCost = FOUR_PLUS_TOP
    return toppingCost

#This function uses the constant DELIVERY_COST if the customer requires delivery, if not it sets it to zero
def deliveryCostCalc(deliveryYorN):
    while True:
        if deliveryYorN.lower() == "yes":
            deliveryCost = DELIVERY_COST
            break
        elif deliveryYorN.lower() == "no":
            print("No delivery cost added.")
            deliveryCost = 0
            break
        else:
            print("Invalid data")
            deliveryYorN = str(input("Does the customer require delivery: "))
    return deliveryCost

#This validates the quantity of pizza, as the restaurant only allows for 1-6 pizza orders
def validatePizzaQuantity(pizzaQuantity):
    while True:
        if 6 >= pizzaQuantity > 0:
            break
        else:
            print("Maximum pizzas allowed is six, try again.")
            pizzaQuantity = input("Please re-enter the number of pizzas: ")
    return pizzaQuantity

#The toppingFinder function uses toppingYorN from the main function and checks if it is yes or no, if it isnt yes or no then it asks the user to re-enter the value, after which it repeats
def toppingFinder(toppingYorN):
    while True:
        if toppingYorN.lower() == "yes":
            toppingNum = input(f"Please enter the amount of extra toppings on the pizza: ")
            toppingCost = toppingNumber(toppingNum)
            print("--------------------------------------------------------------------------------------------------------------------------------------------------------------------")
            break 
        elif toppingYorN.lower() == "no":
            print("Extra topping cost will be set to 0.")
            print("--------------------------------------------------------------------------------------------------------------------------------------------------------------------")
            toppingCost = 0
            break
        else: 
            print("Invalid response.")
            toppingYorN = str(input("Please enter 'Yes' or 'No': "))
    return toppingCost

#Main programme
def main():
  #Customer details which the user inputs
    name = str(input("Input is the customer's name: "))
    address = str(input("Input the customer's address: "))
    phoneNum = int(input("Input the customer's phone number: "))
    print("--------------------------------------------------------------------------------------------------------------------------------------------------------------------")
  #Puts pizza quantity into the function to validate it
    pizzaQuantity = input("Enter amount of pizzas ordered (1-6): ")
    pizzaQuantity = validatePizzaQuantity(pizzaQuantity)
    print("--------------------------------------------------------------------------------------------------------------------------------------------------------------------")

    totalCost = 0
  #These two lists are used to store the data to then be put into the customers bill
    costOfPizzas = []
    sizeOfPizzas = []
    i = 1
    
  #For loop acting as the main initiator to the rest of the function, it does most of the calculations
    for i in range(1, pizzaQuantity + 1):
        sizePizza = input(f"Enter the size of pizza {i} (Small, Medium or Large): ")
        pricePizza = sizePizzaFind(sizePizza)
        toppingYorN = str(input("Are you adding extra toppings (Yes or No): "))
        toppingCost = toppingFinder(toppingYorN)
        pizzaCost = pricePizza + toppingCost
        #After each loop it adds the size, and cost of the pizza to a list
        sizeOfPizzas.append(sizePizza)
        costOfPizzas.append(pizzaCost)

  #This adds up the cost of the pizza in the list without the discount for over £20 pound orders
    baseTotalCost = sum(costOfPizzas)
    deliveryYorN = str(input("Does the customer require delivery: "))
    deliveryCost = deliveryCostCalc(deliveryYorN)
    totalCost= baseTotalCost + deliveryCost
    q = 1
    print("-----------Order summary-----------")
    print(f"The following bill is for customer {name}, address {address} and phone number {phoneNum}.")
    
  #This for loop goes up the range of the cost of the pizza (also the size of pizzas but they are the same length), printing the first value in those lists,
  #it then increases the value of q so the programme will print (for example. For the 1 small pizza the cost is 3.25)
    for q in range(len(costOfPizzas)):
        print(f"For the {q+1} {sizeOfPizzas[q].lower()} pizza the cost is £{costOfPizzas[q]:.2f}.")
        q =+ 1
    print(f"The delivery charge is £{deliveryCost:.2f}.")
    print(f"The total cost is £{totalCost:.2f}.")
    
  #If the cost is above £20 the customer gets a 10% discount on their total cost, which is applied after
    if baseTotalCost > 20:
        discountCost = totalCost*DISCOUNT
        finalCost = totalCost - discountCost
        print(f"Since the customer spent over £20 they qualify for a 10% discount.")
        print(f"The final cost will now be £{finalCost:.2f}.")

#Initiating the main programme starting my code
main()
