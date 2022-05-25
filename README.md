# Shopify-Technical-assignment
Fall 2022 Data Science Intern Challenge 



Question 1:  The googlecolab link : https://colab.research.google.com/drive/1Y5F2VFsuoDLzHrolXMiznKZI2XJ6ycmC?usp=sharing

Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 
After doing some Data exploratory and using the statistics, the AOV is considered as the mean value of order_amount columns which is the average value of all orders. By looking at the distribution, the max numbers (outliers) are far from the mean and the quartiles. Also, there is a large gap between the mean and the quartiles, while the quartiles are close to one another, which confirms the existence of the outliers.


By looking at the statistics above we can identify two types of outliers that can make the AOV number meaningless. the 1st type is bulk purchases as seen from the stats of the total_items(the max is much larger than the higher quartile),the 2nd type of outliers are the orders with a very large “average price per item in each order” (av : as each shop sells one type of sneakers it is the price of the sneaker for each shop), as seen from the stats of av -the max is much larger than the higher quartile.- which can indicate suspicious sales since the sneaker prices are a lot lower than $25000.

The mean is not a good metric for evaluating this dataset as it is highly sensitive to outliers. Instead, mode (most frequently occurring value) and median (the middle value) could be more representative of order values. 
Original Removing outlier:(section 2 in G colab)

Mean of Order_amounts
mode of Order_amounts
median of Order_amounts
3145.128
153
284



Another way is to set aside the outliers and calculate the statistic of the remaining dataset. The mean might be useful then.
This part is done in sections 3 and 4 in google colab link. Some thresholds are chosen and set. The outliers are mostly for shop_id 78 and 42. finally, we have the below statistics description:

We can see that the quartiles, min, max, and mean are much better and reasonable. The mean is near the median(50% quartile). Also, the median and mode have not been changed by removing the outlier.
This confirms the low sensitivity of these metrics to the 

AFER Removing outlier:(section 4 in G colab)

Mean of Order_amounts
mode of Order_amounts
median of Order_amounts
300.15
153
284



What metric would you report for this dataset? The mode as it has outliers


What is its value?153


Question 2: For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

How many orders were shipped by Speedy Express in total?
	
Code: 
select count(OrderID) from Orders where ShipperID=(select ShipperID from Shippers where ShipperName='Speedy Express')
Answer: 54

What is the last name of the employee with the most orders?
Code :
SELECT LastName FROM (select max(x.cnt), EID from (select count() as cnt ,EmployeeID as EID from Orders group by EmployeeID) x)y JOIN Employees on y.EID = Employees.employeeID 
Answer = Peacock
What product was ordered the most by customers in Germany?

           Code:
SELECT ProductName FROM Products where ProductID =(select ProductID from (select sum(Quantity) as total_quantity ,ProductID
 from OrderDetails where OrderID in(select OrderID from Orders
where CustomerID in
(SELECT CustomerID FROM Customers where Country ='Germany')) group by ProductID order by total_quantity DEsc limit 1))
Answer :Boston Crab Meat

