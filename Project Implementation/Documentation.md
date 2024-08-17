# <img src="https://cdn-icons-png.flaticon.com/512/353/353288.png" width="5%" height="5%"> Wholesale Store: Orders Data ETL Phase-wise Implementation

---

## Implementation Steps
- [1. Configuring the Inputs](#1-configuring-the-inputs)
- [2. Preview the loaded Input data](#2-preview-the-loaded-input-data)
- [3. Data Validation](#3-data-validation)
- [4. Data Transformation for Task 1 & 2](#4-data-transformation-for-task-1--2)
- [5. Data Export for Task 1 & 2](#5-data-export-for-task-1--2)
- [6. Data Transformation for Task 3](#6-data-transformation-for-task-3)
- [7. Data Export for Task 3](#7-data-export-for-task-3)

---

## 1. Configuring the Inputs:

- Imported the data from the 4 Excel file data sources - Orders, Customers, POS Orders and Returns using the Input Data tool.
- Imported the data for the US State Codes using the Text Input Tool and entering the data: 1. Alabama	AL, 2. Alaska	AK, 3. Arizona	AZ, 4. Arkansas	AR, 5. California	CA, 6. Colorado	CO, 7. Connecticut	CT, 8. Delaware	DE, 9. Florida	FL, 10. Georgia	GA, 11. Hawaii	HI, 12. Idaho	ID, 13. Illinois	IL, 14. Indiana	IN, 15. Iowa	IA, 16. Kansas	KS, 17. Kentucky	KY, 18. Louisiana	LA, 19. Maine	ME, 20. Maryland	MD, 21. Massachusetts	MA, 22. Michigan	MI, 23. Minnesota	MN, 24. Mississippi	MS, 25. Missouri	MO, 26. Montana	MT, 27. Nebraska	NE, 28. Nevada	NV, 29. New Hampshire	NH, 30. New Jersey	NJ, 31. New Mexico	NM, 32. New York	NY, 33. North Carolina	NC, 34. North Dakota	ND, 35. Ohio	OH, 36. Oklahoma	OK, 37. Oregon	OR, 38. Pennsylvania	PA, 39. Rhode Island	RI, 40. South Carolina	SC, 41. South Dakota	SD, 42. Tennessee	TN, 43. Texas	TX, 44. Utah	UT, 45. Vermont	VT, 46. Virginia	VA, 47. Washington	WA, 48. West Virginia	WV, 49. Wisconsin	WI, 50. Wyoming	WY, 51. American Samoa	AS, 52. Guam	GU, 53. Northern Mariana Islands	MP, 54. Puerto Rico	PR, 55. Virgin Islands	VI
- Encased all the above 5 data sources in individual Tool Containers titled: Data Warehouse - ORDERS, Data Warehouse - CUSTOMERS, POS Orders Data, Returns and US State Codes Reference List respectively and added descriptive comments. Added a master Tool Container for all data sources titled ‘DATA INPUTS’.
- Since many of these inputs have almost a million records, I'd like to cache all of these inputs since I will want to be able to run this workflow a number of times just for testing purposes. This will have to be done one by one. So I'll just right click any one of the input tools and then click cache and run workflow, but to prevent running the other inputs as well I will disable the tool containers for other inputs so that not all inputs will run. I can leave the last text input enabled since it is a manual data input and that's really not going to slow down my workflow. So I'll leave that enabled. Cached inputs will be denoted by  a blue bubble. Repeat for all the other inputs.

## 2. Preview the loaded Input data:

- The existing Orders table has an ORDER_ID field which is presumably the Primary Key. It also has other fields for product information like product_sku, quantity, order_date, customer_id, returned, product_name, product_desc, product_price and product_category.
- The Point of Sale (POS) orders data are the new orders that have been placed in August. They also have an ORDER_ID as well as presumably the Primary Key, and the order date should all be August. The rest of the columns seem to be the same with the addition of some customer columns that I didn't see in the data warehouse orders table, which makes sense because in the data warehouse the data there is split into different tables. So there's a separate table with customer info.
- The Returns is a simple data set where we can see here the ORDER_ID field and then whether that order was returned or not. These are all returns received in August 2021 and the order could have been placed before August 2021.
- The existing Customers table has an ID field which is presumably the Primary Key. It also has other fields for customer information like FirstName, LastName, UserName, Email, State, City, Street, PostCode and Phone.

## 3. Data Validation:
Add a select tool to all the input tools to validate the datatypes and field name and to make changes if necessary.

- For Existing Orders table, change the CUSTOMER_ID field datatype to V_String similar to the ORDER_ID field. All other fields were validated correctly.
- For POS Orders table, change the QUANTITY field datatype to Double, ORDER_DATE field datatype to DateTime and PRODUCT_PRICE field datatype to Double. All other fields were validated correctly.
- For Existing Customers table, change the ID field datatype to V_String. All other fields were validated correctly.
- For Returns table, all fields were validated correctly.
- Add a ‘Fix data types’ annotation to all the Select tools used for the above validations.
- Encase all the above select tools in a Tool Container titled ‘DATA VALIDATION’.

## 4. Data Transformation for Task 1 & 2:
Next I will join the POS Orders table and the Returns table to mark any item that has been placed as an order in August as returned if it appears in this return list. I’ll also have to do the same with the Existing Orders table as well.

- There are a lot of customer information fields in the POS Orders table that are not present in the Existing Orders table. So I'll remove them from the POS Orders dataset as well since I’ll not be needing it using an additional Select tool and unchecking all of the customer detail fields except of course CUSTOMER_ID so I can always join it back later if so needed. 
Add a ‘Remove Customer detail fields’ annotation to this Select tool.
- Connect the 2nd Select tool output of the POS Orders table to the L Input anchor and the Returns table to the R Input anchor of the Join tool. Join based on the ORDER_ID field and unselect the duplicate ORDER_ID field from Returns table to not be shown in the output. Rerun the workflow. Now in the L Left Output I get orders that have not been returned so the Returned column is not present here and in the J Join Output are all orders which have indeed been returned and so I see the Returned column at the end here with True value.
- I want to add the Returned column to this L Left Output as well so that I have the same number of columns in my data set irrespective of Join type. For this I used Formula tool to add a new Returned column and set its value to Boolean datatype with value as 0 i.e False. Add a ‘Set RETURNED field to False’ annotation to this Formula tool.
- Now I want to combine all the 3 outputs using the Union tool to make it one data set again. Set the Join tool L Output after the Formula tool connection line label to ‘August Orders - Not Returned’. Set the Join tool J Output connection line label to ‘August Orders - August Returned’. Annotate Union tool as ‘Union: August Orders’.
- The Join tool R Right Output contains orders which have been returned as well but they weren't placed in August. And so I’ll have to join that with the Existing Orders table using a Join tool. Connect the Existing Orders table Select tool output to the L Input anchor and label the connection line as ‘Prior August Orders’. Connect the R Right Join Output anchor of the above Join tool to the R Input anchor of this Join tool and label the connection line as ‘Prior August Orders - Returned’. Join based on the ORDER_ID field and unselect the duplicate ORDER_ID field from previous Join tool R Output to not be shown in the output. I also have a duplicate for the Returned column now because the Returned column already exists in our data warehouse but I decided we need both because an order might have already been marked as a return in the data warehouse and we don't wanna alter that if it's not been returned in August. So essentially eventually I will have to merge those two columns. Rerun the workflow. Now in the L Left Output is data which has not been returned in August. It might have been returned before, but definitely not in August. The J Join Output contains all the data that was actually returned in August and in the R Right Output I have nothing since we've either used the order data for the August orders or we've used it for orders placed prior to August.
- The J Join Output has 2 Returned fields since when the order was placed prior to August so the Returned value was defaulted to false and when it was returned in August it was set to true. So I don't need the original value of false, but the new value of true and I need the original column name Returned. Using a Select tool I'm going to remove the Returned column, that's the original column with the false values that I don't need anymore. And I'm gonna keep the Right_Returned column, I'll move that up so it's in the same position the original Returned column was and I'm going to rename this to Returned.
- Now I want to combine all the outputs using the Union tool to make it one data set again. Set the Join tool L Output connection line label to ‘Prior August Orders - Not Returned’. Set the Join tool J Output before the Select tool connection line label to ‘Prior August Orders - August Returned’. Annotate Union tool as ‘Union: Prior August Orders’.
- So now I have four data flows: 1. Orders placed prior to August but not returned in August, 2. Orders placed prior to August and returned in August, 3. Orders placed in August and not returned, 4. Orders placed in August and returned. I will combine all of these different flows into one using another Union tool. Annotate Union tool as ‘Union: All Orders’.
- Encase all the above tools in a Tool Container titled ‘Task 1 & 2: Update Existing & New Orders with Return Indicator - DATA TRANSFORMATION’.

## 5. Data Export for Task 1 & 2:
After all the above transformations the data is now ready to be pushed into our data warehouse into the Orders table.

- So normally we should write to a database but since that is not available, I'll just write to an Excel file using a Output Data tool and it will output in the same directory I've saved this particular workflow inside the Data Outputs folder and the file name will be Output - ORDERS.xlsx. Rerun the workflow to export the file. In configuration window, set Output Options to Overwrite the file with each rerun.
- Open the Excel file and cross verify if all the columns have been exported correctly. Encase the above tool in a Tool Container titled ‘Task 1 & 2: DATA OUTPUT’.
- Clear Cached Input Data before making final Orders Data Export.

## 6. Data Transformation for Task 3:
In this case I need to do the opposite to Task 1 & 2, I have to drop the orders details which is not relevant instead of customer details.

- There are a lot of product information fields in the POS Orders table that are not present in the Existing Customers table. So I'll remove them from the POS Orders dataset as well since I’ll not be needing it using an additional Select tool and unchecking all of the product detail fields except of course CUSTOMER_ID and other customer detail fields. Also rename the CUSTOMER_ID field as ID so its consistent with the Existing Customers table.
Add a ‘Remove Order detail fields’ annotation to this Select tool.
- I’ll also replace the State field with State codes from the Text Input to match consistency with the Existing Customers table using the Find & Replace tool. Connect the Select tool above with the F Find Input anchor and the State Code Text Input with the R Replace Input anchor. In the configuration window, I want to match an entire field that is the state in our matching data set, I want to find the State value as well and if I find state in our POS data, I want to replace the found text with the State_Abbreviated column from the reference file. Rerun the workflow to replace the values. Annotate Find & Replace tool as ‘Replace by State Code’.
- We want the Data Warehouse to be updated with the most recent customer details. To achieve this I need to join both the Customers from August POS Orders and Existing Customers using a Join tool. Connect the Find & Replace tool above output to the L Left input anchor and Existing Customers input to the R Right input anchor of the Join tool. Label the Left input anchor connecting line as ‘August Customers’ and the Right input anchor connecting line as ‘Existing Customers’.
- Join based on the ID field. Rerun the workflow. On the L Left Join output I get no results which makes sense because a customer has to be registered in order to place an order. So if that wasn't the case, new customers who placed an order for the first time in August would appear here. In the J Join output I have customers that have placed an order in August as well as before August. So an existing customer placing an order in August. In the R Right Join output are existing customers who did not place an order in August.
- All the extra duplicate fields on the end that the Join output brought in with prefix ‘Right_’, are populated with the old customer data that now needs to be updated in the Existing Customers table. For this we’ll uncheck all of these fields with a ‘Right_’ prefix since they are old details coming in from Existing Customers table and are no longer relevant as we have new customer details data coming from the POS Orders table. Rerun the workflow.
- We’ll combine all the customers from the J Join and Right Join outputs together using a Union tool. Label the connecting line from the J Join as ‘Updated Customer details’ and from the Right Join as ‘Unchanged Customer details’. Rerun the workflow. Annotate Union tool as ‘Union: All Customers’.
- However since no new customers were introduced. The customer count in Existing Customers table 985,857 should be same as that after the Union of all customers which comes out to be 1,046,050. This indicates the presence of duplicates in the POS Orders dataset. This anomaly is happening since POS Orders has multiple records for the same customer for different orders. This can be fixed by adding a Unique tool to filter based on unique ID field went we introduce the POS Orders data input before the Find & Replace tool. Rerun the workflow. Now the Union tools outputs the correct 985,857 unique customers. Annotate Unique tool as ‘Unique: ID’.
- Encase all the above tools in a Tool Container titled ‘Task 3: Update Customer Details - DATA TRANSFORMATION’.

## 7. Data Export for Task 3:
After all the above transformations the data is now ready to be pushed into our data warehouse into the Customers table.

- So normally we should write to a database but since that is not available, I'll just write to an Excel file using a Output Data tool and it will output in the same directory I've saved this particular workflow inside the Data Outputs folder and the file name will be Output - CUSTOMERS.xlsx. Rerun the workflow to export the file. In configuration window, set Output Options to Overwrite the file with each rerun.
- Open the Excel file and cross verify if all the columns have been exported correctly. Encase the above tool in a Tool Container titled ‘Task 3: DATA OUTPUT’.
- Clear Cached Input Data before making final Customers Data Export.

---
