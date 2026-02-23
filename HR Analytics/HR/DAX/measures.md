DEFINE
    MEASURE '_Measures'[TotalEmployees] = DISTINCTCOUNT(DimEmployee[EmployeeID])
    MEASURE '_Measures'[ActiveEmployees] = CALCULATE(
    COUNT(
        DimEmployee[Attrition]),
        DimEmployee[Attrition] = "No")
    MEASURE '_Measures'[InactiveEmployees] = CALCULATE(
    COUNT(
        DimEmployee[Attrition]),
        DimEmployee[Attrition] = "Yes")
    MEASURE '_Measures'[% Attrition Rate] = [InactiveEmployees]/[TotalEmployees]
    MEASURE '_Measures'[TotalEmployeesDate] = CALCULATE([TotalEmployees],
USERELATIONSHIP(DimEmployee[HireDate], DimDate[Date]))
    MEASURE '_Measures'[AverageSalary] = AVERAGE(DimEmployee[Salary])
    MEASURE '_Measures'[LastReviewDate] = VAR _LastDate = MAX(FactPerformanceRating[ReviewDate])
RETURN
IF(ISBLANK(_LastDate), "No Review",
FORMAT(_LastDate, "mm/dd/yyyy"))
    MEASURE '_Measures'[NextReviewDate] = VAR _LastReview = 
IF(
    MAX(FactPerformanceRating[ReviewDate]) = BLANK(),
    MAX(DimEmployee[HireDate]),
    MAX(FactPerformanceRating[ReviewDate]))

RETURN FORMAT(_LastReview + 365, "mm/dd/yyyy")
    MEASURE '_Measures'[JobSatisfaction] = MAX(FactPerformanceRating[JobSatisfaction])
    MEASURE '_Measures'[EnvironmentSatisfaction] = CALCULATE(MAX(FactPerformanceRating[EnvironmentSatisfaction]), USERELATIONSHIP(DimSatisfiedLevel[SatisfactionID], FactPerformanceRating[EnvironmentSatisfaction]))
    MEASURE '_Measures'[RelationshipSatisfaction] = CALCULATE(MAX(FactPerformanceRating[RelationshipSatisfaction]), USERELATIONSHIP(DimSatisfiedLevel[SatisfactionID], FactPerformanceRating[RelationshipSatisfaction]))
    MEASURE '_Measures'[WorkLifeBalance] = CALCULATE(MAX(FactPerformanceRating[WorkLifeBalance]), USERELATIONSHIP(DimSatisfiedLevel[SatisfactionID], FactPerformanceRating[WorkLifeBalance]))
    MEASURE '_Measures'[SelfRating] = CALCULATE(MAX(FactPerformanceRating[SelfRating]), USERELATIONSHIP(DimRatingLevel[RatingID], FactPerformanceRating[SelfRating]))
    MEASURE '_Measures'[ManagerRating] = CALCULATE(MAX(FactPerformanceRating[ManagerRating]), USERELATIONSHIP(DimRatingLevel[RatingID], FactPerformanceRating[ManagerRating]))
    MEASURE '_Measures'[InactiveEmployeesDate] = CALCULATE([InactiveEmployees], USERELATIONSHIP(DimDate[Date], DimEmployee[HireDate]))
    MEASURE '_Measures'[% Attrition Rate Date] = [InactiveEmployeesDate]/[TotalEmployeesDate]

EVALUATE
    SUMMARIZECOLUMNS(
        "TotalEmployees", [TotalEmployees],
        "ActiveEmployees", [ActiveEmployees],
        "InactiveEmployees", [InactiveEmployees],
        "% Attrition Rate", [% Attrition Rate],
        "TotalEmployeesDate", [TotalEmployeesDate],
        "AverageSalary", [AverageSalary],
        "LastReviewDate", [LastReviewDate],
        "NextReviewDate", [NextReviewDate],
        "JobSatisfaction", [JobSatisfaction],
        "EnvironmentSatisfaction", [EnvironmentSatisfaction],
        "RelationshipSatisfaction", [RelationshipSatisfaction],
        "WorkLifeBalance", [WorkLifeBalance],
        "SelfRating", [SelfRating],
        "ManagerRating", [ManagerRating],
        "InactiveEmployeesDate", [InactiveEmployeesDate],
        "% Attrition Rate Date", [% Attrition Rate Date]
    )