> This file contains my solutions to the scenarios found in Scott Simpson's "SQL Code Challenges" course on LinkedIn Learning
> This file is limited to the queries related to the Restaurant database found under Exercise Files in the course

#1
Create invitations for a party <br><br>
Create a list of customers to send invitations to, sorted alphabetically by last name
```
SELECT FirstName, LastName, Email
FROM Customers
ORDER BY LastName;
```

#2
Create a new table to track anniversary attendees & party size based on customer ID
```
CREATE TABLE AnniversaryAttendees ("CustomerID" INT, "PartySize" INT);
```

#3
Create 3 custom menus <br><br>
Output all items from Dishes table, sorted by price low to high
```
SELECT *
FROM Dishes
ORDER BY Price ASC;
```

Output custom menu, with all appetizer & beverage options, sorted by type
```
SELECT *
FROM Dishes
WHERE Type = "Appetizer" OR Type = "Beverage"
ORDER BY Type;
```
Output custom menu, with all items except beverages, sorted by type
```
SELECT *
FROM Dishes
WHERE Type != "Beverage"
ORDER BY Type;
```

#4
Add customer information to Customers table in database
```
INSERT INTO Customers (FirstName, LastName, Email, Address, City, State, Phone, Birthday)
VALUES("Michelle", "Coddington", "michelle@nomail.com", "1021 Charla Lane", "Dallas", "TX", "(972) 555-3128", "1971-07-30");
```
Verify record was added
```
SELECT *
FROM Customers
ORDER BY CustomerID DESC;
```

#5
Update existing customer's mailing address in Customers table <br><br>
Find correct customer in database
```
SELECT CustomerID, FirstName, LastName, Address
FROM Customers
WHERE FirstName = "Taylor" AND LastName = "Jenkins";
```

Update customer information
```
UPDATE Customers
SET Address = "74 Pine St.", City = "New York", State = "NY"
WHERE CustomerID = 26;
```

Verify update was correct
```
SELECT *
FROM Customers
WHERE CustomerID = 26;
```

#6
Delete customer from Customers table <br><br>
Find correct customer in database
```
SELECT CustomerID, FirstName, LastName, Email
FROM Customers
WHERE FirstName = "Taylor" AND LastName = "Jenkins";
```

Delete customer record
```
DELETE FROM Customers WHERE CustomerID = 4;
```

#7
Log customer responses in earlier created table
```
INSERT INTO AnniversaryAttendees
	(CustomerID, PartySize)
	VALUES
	((SELECT CustomerID FROM Customers WHERE Email = "atapley2j@kinetecoinc.com"), 4);
 ``` 
 
#8
Find a reservation in the Reservations table without knowing correct spelling of name, join the Customers table to find name
```
SELECT Customers.FirstName, Customers.LastName, Reservations.Date, Reservations.PartySize
FROM Reservations
JOIN Customers ON Customers.CustomerID = Reservations.CustomerID
WHERE Customers.LastName LIKE "Ste%";
```

#9
Create a reservation in the Reservations table, using the Customer table to look up personal information <br><br>
Look up customer by email address
```
SELECT * FROM Customers WHERE Email = "smac@rouxacademy.com"
```

Create new customer in Customers table
```
INSERT INTO Customers(FirstName, LastName, Email, Phone)
VALUES("Sam", "McAdams", "smac@rouxacademy.com", "(555) 555-1212");
```

Find new customer's generated CustomerID
```
SELECT * FROM Customers ORDER BY CustomerID DESC;
```

Add reservation
```
INSERT INTO Reservations(CustomerID, Date, PartySize)
VALUES("102", "2020-07-14 18:00:00", "5");
```

#10
Create a delivery order <br><br>
Find the customer in Customers table
```
SELECT * 
FROM Customers
WHERE FirstName = "Loretta" AND LastName = "Hundey";
```

Create order using CustomerID
```
INSERT INTO Orders(CustomerID, OrderDate)
VALUES("70", "2020-07-31 16:00:00");
```

Look up generated OrderID
```
SELECT * FROM Orders WHERE CustomerID = 70 ORDER BY OrderDate DESC;
```

Add items to order using OrderDishes table
```
INSERT INTO OrdersDishes(OrderID, DishID)
VALUES
	("1001", (SELECT DishID FROM Dishes WHERE Name="House Salad")),
	("1001", (SELECT DishID FROM Dishes WHERE Name="Mini Cheeseburgers")),
	("1001", (SELECT DishID FROM Dishes WHERE Name="Tropical Blue Smoothie"));
```
  
Return sum of ordered items
```
SELECT SUM(Dishes.Price)
FROM Dishes 
JOIN OrdersDishes ON Dishes.DishID = OrdersDishes.DishID
WHERE OrdersDishes.OrderID = 1001;
```

#11
Update customer's favorite dish in Customers table <br><br>
Find correct CustomerID
```
SELECT * FROM Customers
WHERE FirstName = "Cleo" AND LastName = "Goldwater"
```

Find DishID in Dishes table
```
SELECT DishID
FROM Dishes
WHERE Name="Quinoa Salmon Salad"
```

Update customer's favorite dish using CustomerID
```
UPDATE Customers
SET FavoriteDish = 9
WHERE CustomerID = 42
```

#12
Generate report of top 5 customers with most to-go orders
```
SELECT Customers.FirstName, Customers.LastName, Customers.Email, COUNT(Orders.CustomerID) AS no_of_orders
FROM Customers
JOIN Orders ON Customers.CustomerID = Orders.CustomerID
GROUP BY Orders.CustomerID
ORDER BY no_of_orders DESC
LIMIT 5;
```
