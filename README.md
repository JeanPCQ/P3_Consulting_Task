
# P3 Consulting Task
Your mission, should you choose to accept it, is to use the data contained within the attached Excel file, to create a Power BI data model using industry best practices to solve our simulated consulting project business challenge and to create a demo report. Here are the specific details & requirements of the mission:
- Please recreate the screenshotted legacy report from the Excel file.
-	We will evaluate your understanding of the DAX language and modeling in creating these metrics.
-	In the future, the client wants to add additional data to analyze along with the sales data and will desire time intelligence calculations. Please consider this in the design of your data model.
-	Lastly, impress the client by designing an additional 1–2-page report demo highlighting your capabilities in terms of both data analysis and report design. 


## Step 1: Creating a Connection
A data connection is essential to pull, manage, and utilize information effectively. Here are some common methods, each with distinct advantages and drawbacks. For this instance, we are going to use Action 3. 

1) Upload the Data to a Server
    - <b>Pros:</b> Ideal for handling daily reports, as uploading data to a server allows for easier data management and sharing across teams. Servers provide a centralized location, simplifying data access and organization.
    - <b>Cons:</b> Setting up automation is essential for efficiency. Using Python to automate report reading and data uploads can save time and reduce manual errors.

2) Upload the File to the Cloud
    - <b>Pros:</b> Cloud storage is easily accessible and enables data sharing and collaboration. Files can be updated in real-time, making it convenient for dynamic reports or regularly updated datasets.
    - <b>Cons:</b> Since multiple users can access and edit the data, it’s crucial to manage permissions carefully to avoid accidental errors or unauthorized changes.

3) Save the File in a Local Folder
    - <b>Pros:</b> Local storage provides control over data accessibility and is easy to navigate. It works well for personal projects or sensitive data that doesn’t require broad access.
    - <b>Cons:</b> Data stored locally can only be viewed on your device unless you share the entire folder and set up specific access paths for other users. This can limit collaboration and increase setup complexity.


## Step 2: Tackeling the Task

1) Recreate the screenshotted legacy report from the Excel file.
     - Column 1............Contains the month and year of first purchase. We have to create a new column and Format the date to "MMM-YY".
     - Column 2............Contains Unique Customer. No Dax is required, simply select the key and select "Count Distinct".
     - Column 3/4........Gives you the percentage of customers returning after their first initial purchase, 0 to 90, and 91-180. I used a Dax Formula to give you that information.  
```
DistinctCustomersWithin90Days = 
VAR MSRMENT =
    CALCULATE(
        DISTINCTCOUNT(Sales[CustomerKey]),
        FILTER(
            Sales,
            DATEDIFF(
                RELATED(Customers[DateFirstPurchase]), 
                Sales[OrderDate], 
                DAY
            ) >= 0 &&
            DATEDIFF(
                RELATED(Customers[DateFirstPurchase]), 
                Sales[OrderDate], 
                DAY
            ) <= 90 &&
            RELATED(Customers[DateFirstPurchase]) <> Sales[OrderDate] // Ensure FirstPurchaseDate is not equal to OrderDate
        )
    )
RETURN
       IF(
        ISBLANK(MSRMENT), 
        0, 
        DIVIDE(DISTINCTCOUNT(Customers[AltCustomerKey]), MSRMENT)/100 // Divide distinct count of AltCustomerKey by MSRMENT
    ) // Return 0 if MSRMENT is blank, otherwise return MSRMENT
```

2) In the future, the client wants to add additional data to analyze along with the sales data and will desire time intelligence calculations. Please consider this in the design of your data model.
    - A Calendar Date is needed to make this work. I decided to use the MIN & MAX of the 'Customers'[DateFirstPurchase] to ensure we have data to showcase.  
```
CalendarTable = 
CALENDAR(
    MIN('Customers'[DateFirstPurchase]),
    MAX('Customers'[DateFirstPurchase])
)
```

3) Lastly, impress the client by designing an additional 1–2-page report demo highlighting your capabilities in terms of both data analysis and report design. 


