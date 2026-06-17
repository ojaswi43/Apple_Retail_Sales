# Apple_Retail_Sales
![Logo](https://github.com/ojaswi43/Apple_Retail_Sales/blob/main/Apple_store.jpeg)
# Project Overview
This project demonstrates advanced SQL querying skills through the analysis of Apple retail sales records. Using data on products, stores, sales transactions, and warranty claims across global locations, I solved a range of business problems and extracted actionable insights using complex SQL techniques.
# Schema
The project uses five main tables:

Stores: Data about Apple retail stores.
Store_id: Unique identifier for each store.
Store_name: Name of the store.
City: City where the store is located.
Country: Country of the store.

Category: Holds product category information.
Category_ID: Unique identifier for each product category.
Category_Name: Name of the category.

Products: Details about Apple products.
Product_ID: Unique identifier for each product.
Product_Name: Name of the product.
Category_ID: References the category table.
Launch_Date: Date when the product was launched.
Price: Price of the product.

Sales: Stores sales transactions.
Sale_ID: Unique identifier for each sale.
Sale_Date: Date of the sale.
Store_ID: References the store table.
Product_ID: References the product table.
Quantity: Number of units sold.

Warranty: Contains information about warranty claims.
Claim_id: Unique identifier for each warranty claim.
Claim_date: Date the claim was made.
Sale_id: References the sales table.
Repair_status: Status of the warranty claim (e.g., Paid Repaired, Warranty Void).
