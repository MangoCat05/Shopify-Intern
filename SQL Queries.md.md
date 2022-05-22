
**a.	How many orders were shipped by Speedy Express in total?**

        WITH CTE AS (
    SELECT * 
    FROM Orders O
    LEFT JOIN Shippers S on O.ShipperID = S.ShipperID)
    
    SELECT CTE.ShipperName, SUM(O1.Quantity) AS Total_Quantity
    FROM CTE
    LEFT JOIN OrderDetails O1 on CTE.OrderID = O1.OrderID
    WHERE ShipperName = "Speedy Express"

**ANSWER**
| ShipperName | Total_Quantity |
|--|--|
|Speedy Express  | 3575 |


**b.	What is the last name of the employee with the most orders?**

    WITH CTE AS(
    SELECT *, SUM(Quantity) AS Total_Quantity
    FROM Orders O
    LEFT JOIN OrderDetails OD on O.OrderID = OD.OrderID
    GROUP BY EmployeeID
    ORDER BY Total_Quantity DESC)
    
    SELECT E.EmployeeID, E.LastName, CTE.Total_Quantity
    FROM CTE
    LEFT JOIN Employees E on E.EmployeeID = CTE.EmployeeID

**ANSWER**
EmployeeID| LastName |Total_Quantity  |
|--|--|--|
4|Peacock  |3232  |


**c.	What product was ordered the most by customers in Germany?**

    WITH TEMP AS(
    	WITH CTE AS(
    		SELECT *, SUM(Quantity) AS Total_Quantity
    		FROM Orders O
    		LEFT JOIN OrderDetails OD on O.OrderID = OD.OrderID
    		GROUP BY ProductID
    		ORDER BY Total_Quantity DESC)
    
    		SELECT C.CustomerID, C.Country, CTE.OrderID, CTE.ProductID, CTE.Total_Quantity
    		FROM CTE
    		LEFT JOIN Customers C on C.CustomerID = CTE.CustomerID
    		WHERE Country = "Germany")
    
    SELECT T.Country, T.ProductID, T.Total_Quantity, P.ProductName
    FROM TEMP T
    LEFT JOIN Products P on P.ProductID = T.ProductID

**ANSWER**
|Country  |ProductID  |Total_Quantity|ProductName
|--|--|--|--|
|Germany  |40  |256|Boston Crab Meat






