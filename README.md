# ShopifyInternship
2022 Shopify Technical Internship - Data Science
Jacob Dorfmeister

**Question 1**

**A. Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.**

The AOV of $3145.13 was found by simply just taking the average of all of the order_amount values in the table. We know that sneakers don't typically cost that much, but some statistical data would be nice to prove that this is not an accurate reflection of the data. 
Formula: =AVERAGE(D:D)

Additionally, we can take the standard deviation of these values. The value given is 41282.53. That is VERY high, so we know that something is definitely wrong with our data.

Formula: =STDEV(D:D)

Just by scanning the data, we see that there are some orders that don't make much sense. 
For instance, there are multiple orders (such as order 16) for $704,000. 

We can use a couple of quick formulas to get some useful information about out data, such as the first and third quartiles, min, median, and maxes.

Formula: =MIN(D:D)

Formula: =QUARTILE(D:D, 1)

Formula: =MEDIAN(D:D)

Formula: =QUARTILE(D:D, 3)

Formula: =MAX(D:D)

What we need to do is truncate our data and remove any outliers. We can use a formula to find if any of our order_amount values are outliers. It will display a 1 if the value is an outlier, or a 0 if it is not. Lets put this in column H.

Formula: =if(D2<$O$3-$S$3*1.5,1,IF(D2>$Q$3+$S$3*1.5,1,0))

We then can filter out the values marked with a 1 and get rid of the outliers. Lets use a new row called truncated_values in column I.

Formula: =if(H2=0,D2," ")

_All done!_

**B. What metric would you report for this dataset?**
Because the candlestick chart shows us that the data is still skewed, I would report the median value instead of the average. Because the data is still slightly skewed, the median would be a more accurate metric.

**C. What is its value?**
The median of our new dataset was $280, a much more reasonable price for a pair of sneakers.

**Question 2**

**A. How many orders were shipped by Speedy Express in total?**

_Speedy Express was used 54 times._

SELECT ShipperName, COUNT(O.ShipperID) as Times_Used
FROM Orders O JOIN Shippers S on S.ShipperID = O.ShipperID
WHERE ShipperName = 'Speedy Express'


**B. What is the last name of the employee with the most orders?**

_The employee with the last name Peacock had the most orders. He had 40 orders._

SELECT E.LastName, COUNT(O.OrderID) as Number_Of_Orders
FROM Employees E JOIN Orders O on O.EmployeeID = E.EmployeeID 
GROUP BY E.LastName
ORDER BY Number_Of_Orders DESC
LIMIT 1

**C. What product was ordered the most by customers in Germany?**

_Boston Crab Meat was ordered the most by customers in Germany. 160 times to be exact._

SELECT P.ProductName, SUM(OD.Quantity) as Amount_Ordered
FROM Customers C JOIN Orders O on O.CustomerID = C.CustomerID
JOIN OrderDetails OD on OD.OrderID = O.OrderID
JOIN Products P on P.ProductID = OD.ProductID
WHERE C.Country = 'Germany'
GROUP BY P.ProductName
ORDER BY Amount_Ordered DESC
LIMIT 1

