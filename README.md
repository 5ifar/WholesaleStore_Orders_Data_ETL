# <img src="https://cdn-icons-png.flaticon.com/512/353/353288.png" width="5%" height="5%"> Wholesale Store: Order Data Transformation
This repository serves as my documentation for the WholesaleStore: Order Data Transformation - Alteryx Project.

It showcases my competancy to work with ALteryx Designer and demonstrates my proficiency in essential tools like **In/Out Tools (Browse, Input Data, Output Data & Text Input), Preparation Tools (Select, Filter, Formula, Sort & Unique), Join Tools (Join, Find Replace & Union) and Documentation Tools (Comment & Tool Container)**.

The entire project has been implemented using Alteryx Designer 2024.1 (User) (with AMP Engine enabled).

The project raw data source and data output files have not been uploaded to this repository due to size restrictions, but they can be accessed thorugh the Project Workflow Package that has been uploaded.

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

## Contents:
Please find the resource links for the project below:
- [Project Objective](#project-objective)
- [Tools used & Methodologies implemented](#tools-used)

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
