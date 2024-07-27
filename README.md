# <img src="https://cdn-icons-png.flaticon.com/512/353/353288.png" width="5%" height="5%"> Wholesale Store: Order Data Transformation
This repository serves as my documentation for the WholesaleStore: Order Data Transformation - Alteryx Project.

It showcases my competancy to work with ALteryx Designer and demonstrates my proficiency in essential tools like **In/Out Tools (Browse, Input Data, Output Data & Text Input), Preparation Tools (Select, Filter, Formula, Sort & Unique), Join Tools (Join, Find Replace & Union) and Documentation Tools (Comment & Tool Container)**.

The entire project has been implemented using Alteryx Designer 2024.1 (User) (with AMP Engine enabled).

The project raw data source and data output files have not been uploaded to this repository due to size restrictions, but they can be accessed thorugh the Project Workflow Package that has been uploaded.

---

## Contents:
Please find the sectional links for the project below:
- [Project Objective](#project-objective)
- [Tools used & Methodologies implemented](#tools-used)
- [About the Dataset](#about-the-dataset)
  - [Data Sources](#data-sources)
  - [Data Dictionary](#data-dictionary)
  - [Data Integrity](#data-integrity)

---

## Project Objective:
You’re working as an analyst for a wholesale organization i.e a company selling goods to retailers and maintain their order data and customer data in data warehouse. You need to process order data from a Point of Sale (POS) system, as well as an export from the Order Returns department, and update the Orders and Customers table in the data warehouse. Each order placed in the POS system captures the customer details, the product details, price and quantity. Every month this data needs to be moved to a data warehouse where it is stored for long-term use and is used for reporting purposes.

### Assignment:

1. Update the Existing Data Warehouse Orders table with the latest POS orders, placed in August 2021.
We need to create a workflow to take the data from the POS system for August 2021 and move it into the existing order table in the data warehouse. Since data for the previous periods already exists in this destination, we need to add these new records to the existing data.
2. Update the POS and Existing Data Warehouse Orders table with the returns information i.e set the RETURNED column to the right value.
    - Note that a return received in August may be related to an order placed before August, and so sometimes we need to update an existing record in the data warehouse, and other times we need to update the new data from the POS system.
3. Update the Data Warehouse Customers table with the latest customer information from the POS input i.e a customer may have changed their details, like address (anything that is not the customer ID) during their latest order. We want the Data Warehouse to be updated with the most recent customer details. If a customer placed an order in August, we want to override the existing customer details with the details used during their most recent order.
    - Note that the POS system has the US State name in full. Ensure you change that to the State code prior to loading it into the Data Warehouse Customers table.

Note that we refer to data warehouse tables as orders and customer tables in this project, which would typically be database tables. But since we may not have access to a database for this project, we'll use Excel and CSV files instead.

## Tools used:
1. Alteryx Designer- for Data Import, Data Cleaning, Data Transformation and Data Export process
2. GitHub - for documentation

## Skills & Methodologies implemented:
1. Data Import - Input Data Tool & Text Imput Tool
2. Data Validation - Select Tool
3. Data Transformation - Join Tool, Find Replace Tool, Unique Tool, Formula Tool & Union Tool
4. Data Export - Output Data Tool
5. Documentation

---

## About the Dataset:

### Data Sources:
1. Data Warehouse - ORDERS: (File: Orders Table.xlsx) This is the existing ORDERS table in the data warehouse. It contains all orders placed prior to August 2021.
2. Data Warehouse - CUSTOMERS: (File: CUSTOMERS.xlsx) This is the existing CUSTOMERS table in the data warehouse. It contains details for each customer.
3. POS Orders Data: (File: STORE_TRANSACTIONS_ALL_AUG_2021.csv) This is the Point of Sale (POS) data from August 2021
4. Returns: (File: Completed Returns - August 2021.xlsx) These are returns received in August 2021 (Order could be placed prior or during August 2021)
5. US State Codes Reference List: (File: Text Input) This static lookup list maps US state names to US State codes.

### Data Dictionary:
**1. Orders Table.xlsx: 482793 records | 10 fields**

|Column name|Data type|Description|
|-|-|-|
|ORDER_ID|V_String|Unique Order Id number|
|PRODUCT_SKU|V_String|Unique Product SKU (Stock Keeping Unit) number|
|QUANTITY|Double|Quantity of product ordered|
|ORDER_DATE|DateTime|Date the order was placed|
|CUSTOMER_ID|Double|Unique Customer Id number|
|RETURNED|Bool|Order Return Status|
|PRODUCT_NAME|V_String|Name of product|
|PRODUCT_DESC|V_String|Description of product|
|PRODUCT_PRICE|Double|Price of product|
|PRODUCT_CATEGORY|V_String|Category of product|

**2. CUSTOMERS.xlsx: 985857 records | 10 fields**

|Column name|Data type|Description|
|-|-|-|
|ID|Double|Unique Customer Id number|
|FirstName|V_String|First Name of customer|
|LastName|V_String|Last Name of customer|
|UserName|V_String|User Name of customer|
|Email|V_String|Email of customer|
|State|V_String|State of customer|
|City|V_String|City of customer|
|Street|V_String|Street of customer|
|PostCode|V_String|Postal Code of customer|
|Phone|V_String|Mobile number of customer|

**3. STORE_TRANSACTIONS_ALL_AUG_2021.csv: 67792 records | 18 fields**

|Column name|Data type|Description|
|-|-|-|
|ORDER_ID|V_String|Unique Order Id number|
|PRODUCT_SKU|V_String|Unique Product SKU (Stock Keeping Unit) number|
|QUANTITY|V_String|Quantity of product ordered|
|ORDER_DATE|V_String|Date the order was placed|
|CUSTOMER_ID|V_String|Unique Customer Id number|
|PRODUCT_NAME|V_String|Name of product|
|PRODUCT_DESC|V_String|Description of product|
|PRODUCT_PRICE|V_String|Price of product|
|PRODUCT_CATEGORY|V_String|Category of product|
|FirstName|V_String|First Name of customer|
|LastName|V_String|Last Name of customer|
|UserName|V_String|User Name of customer|
|Email|V_String|Email of customer|
|State|V_String|State of customer|
|City|V_String|City of customer|
|Street|V_String|Street of customer|
|PostCode|V_String|Postal Code of customer|
|Phone|V_String|Mobile number of customer|

**4. Completed Returns - August 2021.xlsx: 718 records | 2 fields**

|Column name|Data type|Description|
|-|-|-|
|ORDER_ID|V_String|Unique Order ID number|
|RETURNED|Bool|Order Return Status|

**5. US State Codes Text Input: 55 records | 2 fields**

|Column name|Data type|Description|
|-|-|-|
|State*|V_String|Name of State|
|State_Abbreviated|V_String|Abbreviation Code of State|

## Data Integrity:
ROCCC Evaluation:
- Reliability: MED - The raw dataset is created and updated by Hendrik Kleine (Udemy Instructor). It has total 4 files. All of them were utilized in the analysis.
- Originality: HIGH - First party provider (Hendrik Kleine, Udemy)
- Comprehensiveness: HIGH - Total 4 Files with a total of around 1.5 Million records were provided. Dataset contains multiple dimension parameters for Customers as well as comprehensive Order transaction data.
- Current: LOW - Dataset contains 2021 data i.e almost 3 years old. So its not very relevant. Any trends observed and insights gained need to be comprehended as a general (not yearly) trend.
- Citation: LOW - No official citation/reference available.

---

## Project Implementation:

### 1. Configuring the Inputs:

- Imported the data from the 4 Excel file data sources - Orders, Customers, POS Orders and Returns using the Input Data tool.
- Imported the data for the US State Codes using the Text Input Tool and entering the data: 1. Alabama	AL, 2. Alaska	AK, 3. Arizona	AZ, 4. Arkansas	AR, 5. California	CA, 6. Colorado	CO, 7. Connecticut	CT, 8. Delaware	DE, 9. Florida	FL, 10. Georgia	GA, 11. Hawaii	HI, 12. Idaho	ID, 13. Illinois	IL, 14. Indiana	IN, 15. Iowa	IA, 16. Kansas	KS, 17. Kentucky	KY, 18. Louisiana	LA, 19. Maine	ME, 20. Maryland	MD, 21. Massachusetts	MA, 22. Michigan	MI, 23. Minnesota	MN, 24. Mississippi	MS, 25. Missouri	MO, 26. Montana	MT, 27. Nebraska	NE, 28. Nevada	NV, 29. New Hampshire	NH, 30. New Jersey	NJ, 31. New Mexico	NM, 32. New York	NY, 33. North Carolina	NC, 34. North Dakota	ND, 35. Ohio	OH, 36. Oklahoma	OK, 37. Oregon	OR, 38. Pennsylvania	PA, 39. Rhode Island	RI, 40. South Carolina	SC, 41. South Dakota	SD, 42. Tennessee	TN, 43. Texas	TX, 44. Utah	UT, 45. Vermont	VT, 46. Virginia	VA, 47. Washington	WA, 48. West Virginia	WV, 49. Wisconsin	WI, 50. Wyoming	WY, 51. American Samoa	AS, 52. Guam	GU, 53. Northern Mariana Islands	MP, 54. Puerto Rico	PR, 55. Virgin Islands	VI
- Encased all the above 5 data sources in individual Tool Containers titled: Data Warehouse - ORDERS, Data Warehouse - CUSTOMERS, POS Orders Data, Returns and US State Codes Reference List respectively and added descriptive comments. Added a master Tool Container for all data sources titled ‘DATA INPUTS’.
- Since many of these inputs have almost a million records, I'd like to cache all of these inputs since I will want to be able to run this workflow a number of times just for testing purposes. This will have to be done one by one. So I'll just right click any one of the input tools and then click cache and run workflow, but to prevent running the other inputs as well I will disable the tool containers for other inputs so that not all inputs will run. I can leave the last text input enabled since it is a manual data input and that's really not going to slow down my workflow. So I'll leave that enabled. Cached inputs will be denoted by  a blue bubble. Repeat for all the other inputs.

### 2. Preview the loaded Input data:

- The existing Orders table has an ORDER_ID field which is presumably the Primary Key. It also has other fields for product information like product_sku, quantity, order_date, customer_id, returned, product_name, product_desc, product_price and product_category.
- The Point of Sale (POS) orders data are the new orders that have been placed in August. They also have an ORDER_ID as well as presumably the Primary Key, and the order date should all be August. The rest of the columns seem to be the same with the addition of some customer columns that I didn't see in the data warehouse orders table, which makes sense because in the data warehouse the data there is split into different tables. So there's a separate table with customer info.
- The Returns is a simple data set where we can see here the ORDER_ID field and then whether that order was returned or not. These are all returns received in August 2021 and the order could have been placed before August 2021.
- The existing Customers table has an ID field which is presumably the Primary Key. It also has other fields for customer information like FirstName, LastName, UserName, Email, State, City, Street, PostCode and Phone.

### 3. Data Validation:
Add a select tool to all the input tools to validate the datatypes and field name and to make changes if necessary.

- For Existing Orders table, change the CUSTOMER_ID field datatype to V_String similar to the ORDER_ID field. All other fields were validated correctly.
- For POS Orders table, change the QUANTITY field datatype to Double, ORDER_DATE field datatype to DateTime and PRODUCT_PRICE field datatype to Double. All other fields were validated correctly.
- For Existing Customers table, change the ID field datatype to V_String. All other fields were validated correctly.
- For Returns table, all fields were validated correctly.
- Add a ‘Fix data types’ annotation to all the Select tools used for the above validations.
- Encase all the above select tools in a Tool Container titled ‘DATA VALIDATION’.



