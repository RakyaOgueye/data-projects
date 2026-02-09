Avg Customer Service Calls = AVERAGE('Databel - Data'[Customer Service Calls])

Avg Extra Data Charges = SUM('Databel - Data'[Extra Data Charges])/[Number of Customers]

Avg Extra International Charges = SUM('Databel - Data'[Extra International Charges])/[Number of Customers]

Churn Rate = [Number of Churned Customers]/[Number of Customers]

Churned = IF('Databel - Data'[Churn Label] =  "Yes" , 1, 0)

Contract Category = 
SWITCH('Databel - Data'[Contract Type],
    "One Year", "Yearly Contract",
    "Two Year", "Yearly Contract",
    "Month-to-Month", "Monthly Contract"
)

Demographics = 
IF('Databel - Data'[Senior] = "Yes", "Senior",
    IF('Databel - Data'[Under 30] = "Yes", "Under 30", "Other")
)

Grouped Consumption = 
SWITCH(TRUE(),
    'Databel - Data'[Avg Monthly GB Download] < 5, "Less than 5 GB",
    'Databel - Data'[Avg Monthly GB Download] < 10, "Between 5 and 10 GB",
    "10 or more GB"
)

Number of Churned Customers = SUM('Databel - Data'[Churned])

Number of Customers = COUNT('Databel - Data'[Customer ID])

Number of Unique Customers = DISTINCTCOUNT('Databel - Data'[Customer ID])
