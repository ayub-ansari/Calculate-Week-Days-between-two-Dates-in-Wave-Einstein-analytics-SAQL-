# Calculate Week Days between two Dates in Wave Einstein analytics (SAQL)
Calculate Week Days between two Dates in Wave (Einstein) analytics +SAQL )

**Requirement:** a SAQL query to find the number of WEEKDAYS between two dates. Specifically, can use this SAQL query in a Compute Expression

**SAQL Expression:** 
Here dataset name is "cases". 
Start date is Created Date of a Case record and End Date is ClosedDate of Case. 

```
q = load "cases";
q = filter q by ClosedDate_sec_epoch != 0;
q = foreach q generate daysBetween(toDate("24-06-1985", "dd-MM-yyyy"), toDate(CreatedDate_sec_epoch)) % 7  as standardDateDiff, daysBetween(toDate(CreatedDate_sec_epoch), toDate(ClosedDate_sec_epoch)) % 7 as targetDatesDiff, Id as CaseId;
q = foreach q generate (case 
when standardDateDiff == 0 then (case targetDatesDiff when 1 then 2 when 2 then 3 when 3 then 4 when 4 then 5 when 5 then 5 when 6 then 5 else 1 end)
when standardDateDiff == 1 then (case targetDatesDiff when 1 then 2 when 2 then 3 when 3 then 4 when 4 then 4 when 5 then 4 when 6 then 5 else 1 end) 
when standardDateDiff == 2 then (case targetDatesDiff when 1 then 2 when 2 then 3 when 3 then 3 when 4 then 3 when 5 then 4 when 6 then 5 else 1 end) 
when standardDateDiff == 3 then (case targetDatesDiff when 1 then 2 when 2 then 2 when 3 then 2 when 4 then 3 when 5 then 4 when 6 then 5 else 1 end) 
when standardDateDiff == 4 then (case targetDatesDiff when 1 then 1 when 2 then 1 when 3 then 2 when 4 then 3 when 5 then 4 when 6 then 5 else 1 end) 
when standardDateDiff == 5 then (case targetDatesDiff when 1 then 0 when 2 then 1 when 3 then 2 when 4 then 3 when 5 then 4 when 6 then 5 else 0 end) 
when standardDateDiff == 6 then (case targetDatesDiff when 1 then 1 when 2 then 2 when 3 then 3 when 4 then 4 when 5 then 5 when 6 then 5 else 0 end) 
else 999 end) as result, targetDatesDiff as targetDatesDiff, CaseId as CaseId;
q = foreach q generate (floor(targetDatesDiff / 7) * 5) + result as WeekDaysOpened, CaseId as CaseId;
q = limit q 2000;
```
