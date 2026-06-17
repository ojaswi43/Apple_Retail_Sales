# Apple_Retail_Sales
![Logo](https://github.com/ojaswi43/Apple_Retail_Sales/blob/main/Apple_store.jpeg)
# Project Overview
This project demonstrates advanced SQL querying skills through the analysis of Apple retail sales records. Using data on products, stores, sales transactions, and warranty claims across global locations, I solved a range of business problems and extracted actionable insights using complex SQL techniques.
# Schema
The project has five main tables:

Stores: Data about Apple retail stores.
 + Store_id: Unique identifier for each store.
 + Store_name: Name of the store.
 + City: City where the store is located.
 + Country: Country of the store.

Category: Holds product category information.
 + Category_ID: Unique identifier for each product category.
 + Category_Name: Name of the category.

Products: Apple product details.
 + Product_ID: Unique identifier for each product.
 + Product_Name: Name of the product.
 + Category_ID: References the category table.
 + Launch_Date: Date when the product was launched.
 + Price: Price of the product.

Sales: Sales transactions of each store.
 + Sale_ID: Unique identifier for each sale.
 + Sale_Date: Date of the sale.
 + Store_ID: References the store table.
 + Product_ID: References the product table.
 + Quantity: Number of units sold.

Warranty: Warranty information about warranty claims.
 + Claim_id: Unique identifier for each warranty claim.
 + Claim_date: Date the claim was made.
 + Sale_id: References the sales table.
 + Repair_status: Status of the warranty claim (e.g., Paid Repaired, Warranty Void).

# Entity Relationship Diagram
![Logo](https://github.com/ojaswi43/Apple_Retail_Sales/blob/main/ERD.png)

# Objectives 
1. Find the number of stores in each country.
  ```sql
  Select Country, COUNT(Store_ID) as Total_Stores
  From Stores
  Group by (Country)
  Order by Total_Stores Desc;
```
2. Calculate the total number of units sold by each store.(Mention Store Name too)
```sql
  Select a.Store_ID, b.Store_Name, SUM(Quantity) as Unit_Sold
  From Sales a
  Join Stores b
  On a.Store_ID = b.Store_ID 
  Group by a.Store_ID, b.Store_Name
  Order by Unit_Sold Desc; 
  ```
3. Identify how many sales occurred in December 2023 in each store.
```sql
  Select Store_ID, COUNT(sale_id) AS Sales_December_2026
  From Sales
  Where Sale_Date >= '2023-12-01' 
  and Sale_Date < '2024-01-01'
  Group by Store_ID
  Order by Sales_December_2026;
  ```
4. Determine how many stores have never had a warranty claim filed.
  ```sql
  Select Count (*) From Stores
  Where Store_id not in (Select Distinct Store_ID 
						   From Sales s
				   			Right Join Warranty w 
								On s.sale_ID = w.sale_ID);
  ```
5. Calculate the percentage of warranty claims marked as "Rejected".
  ```sql
  Select (Round(Count(Case When Repair_Status = 'Rejected' Then 1 End) * 100.0 / Count(*),2)) as Rejected_Percentage
  From Warranty;
  ```
6. Find the average price of products in each category.
  ```sql
  Select P.Category_ID, C.Category_name, Avg(Price) as Avg_Price 
  From Products as P 
  Join Category as C
  On P.Category_ID = C.Category_ID 
  Group by C.Category_name, P.Category_ID
  Order By Avg_Price;
  ```
7. How many warranty claims were filed in 2020?
  ```sql
  Select Count(Claim_ID) as Claim_Filled_2020
  From Warranty 
  Where Claim_Date >= '2020-01-01'
  And Claim_Date < '2021-01-01';
  ```
8. For each store, identify the best-selling day based on highest quantity sold.
   ```sql
   With Daily_Store_Sales as 
                       (Select Store_ID, Sale_Date, Sum(Quantity) as Total_Quantity, Rank() Over(Partition by Store_ID ORDER BY SUM(quantity) DESC) as sales_rank
                        From sales
                        Group by store_id, sale_date)
   Select Store_ID, Sale_Date, Total_Quantity
   From Daily_Store_Sales
   Where Sales_rank = 1
   Order BY Store_id;
    ```
9. Identify the least selling product in each country for each year based on total units sold.
   ```sql
   With Product_Rank 
   as (Select st.Country,p.Product_name,Sum(s.Quantity) as Sold_Quantity, Rank() Over(Partition by st.Country Order by Sum(s.Quantity)) as Ranking
       From Sales s
       Join Stores st
       On s.Store_ID = st.Store_ID
       Join Products p
       On s.Product_id = p.Product_ID
       Group By st.Country,p.Product_Name)
   Select * From Product_Rank
   Where Ranking = 1;
   ```
10. Calculate how many warranty claims were filed within 180 days of a product sale.
   ```sql
   Select s.Sale_date,s.Sale_ID, w.Claim_ID, w.Claim_Date, w.Repair_Status
   From Sales s
   Join Warranty w
   on s.Sale_ID = w.Sale_ID
   Where Datediff(day,sale_date,claim_date) <=180;
   ```
11. Determine how many warranty claims were filed for products launched in the last 3 years.
  ```sql
   Select p.product_ID,p.launch_date,w.Claim_ID, w.Claim_Date, w.Repair_Status
   From Products P
   Join Sales s
   on p.Product_ID = s.product_ID
   Join Warranty w
   on w.sale_id = s.sale_id
   Where Launch_date >= dateadd(year,-3,getdate()); 
   ```
12. Identify the product category with the most warranty claims filed in the last two years.
  ```sql
  Select p.category_id,w.claim_id, count(w.claim_id) as Total_Claims
  From Products p 
  Join Sales s
  on p.product_ID = s.product_ID
  Join Warranty w
  on w.sale_id = s.sale_id
  Where claim_date >= Dateadd(year,-2,getdate())
  Group by p.category_id, w.claim_id
  Order by Total_Claims Desc ;
  ```
13. Determine the percentage chance of receiving warranty claims after each purchase for each country.
  ```sql
  Select st.Country, Count(w.Claim_ID) as Total_Claims, Count(s.quantity) as Total_Sales, (Count(w.Claim_ID) * 100.0) / Count(s.Quantity) as Claim_Percentage
  From Sales s
  JOIN Stores st 
  On st.store_id = s. store_id  
  Join Warranty w
  On s.sale_id = w.sale_id
  Group by st.country
  Order By Claim_Percentage Desc;
  ```
# Business Impact 
 + Enabled identification of high-performing stores and peak sales periods, supporting inventory planning and sales optimization.
 + Highlighted product categories with elevated warranty claims, helping improve quality monitoring and customer satisfaction initiatives.
 + Uncovered regional sales and warranty trends, assisting management in strategic resource allocation and operational planning.
 + Improved reporting efficiency by automating retail performance analysis through reusable SQL-based analytical solutions.
