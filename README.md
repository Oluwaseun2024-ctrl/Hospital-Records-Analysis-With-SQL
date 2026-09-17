# HEALTHCARE DATA ANALYSIS USING SQL
**Patient Demographics, Medical Encounters, Insurance Costs & Procedures Analysis**

**Massachusetts General Hospital — Synthetic Patient Dataset (2011–2022)**

**Table of Contents**
1. Project Overview
2. Dataset Overview
3. Database Structure & Relationships
4. Data Quality Assessment
5. Data Cleaning & Error Correction
6. Feature Engineering
7. Patient Demographic Analysis
8. Medical Encounter Analysis
9. Insurance & Financial Analysis
10. Medical Procedure Analysis
11. Key Findings & Insights
12. Recommendations
13. Conclusion

## PROJECT OVERVIEW
**Project Background**

This project analyzes a synthetic healthcare dataset of approximately 1,000 Massachusetts General Hospital patients from 2011–2022, covering demographics, medical encounters, insurance, and procedures.

**Project Purpose**

To use SQL to assess healthcare data quality and generate insights into patient demographics, healthcare utilization, medical costs, insurance coverage, and procedures.

**Project Objectives**
- Establish relationships between database tables.
- Perform data quality checks and corrections.
- Engineer useful analytical features.
- Analyze patient demographics and healthcare utilization.
- Analyze healthcare costs and insurance coverage.
- Analyze medical procedures and readmission patterns.

**Business Questions**
- Who are the patients being served?
- How do healthcare encounters change over time?
- What are the most common encounter types and diagnoses?
- Which payers account for the highest healthcare costs?
- How much of healthcare costs are covered by insurance?
- What are the major out-of-pocket cost patterns?
- What are the most common and costly procedures?
- What patterns of readmission exist?

**Project Scope**

The analysis covers four tables: Patients, Encounters, Payers, and Procedures, using SQL for database structuring, data quality assessment, feature engineering, and healthcare analysis.

## DATASET OVERVIEW
**Dataset Description**

The dataset is a synthetic healthcare dataset containing approximately 1,000 patient records and their healthcare activities, including demographics, medical encounters, insurance coverage, and procedures.

**Data Source and Coverage**

The dataset represents patients and healthcare activities at Massachusetts General Hospital covering the period 2011–2022.

**Database Tables**

The database consists of four main tables:
- Patients — patient demographic and geographic information.
- Encounters — healthcare encounters, diagnoses, and associated costs.
- Payers — insurance provider information.
- Procedures — medical procedures and their associated costs and conditions.

**Data Dictionary**

The data dictionary defines the fields, data types, and purpose of each column across the four tables. Key fields include patient IDs, encounter IDs, payer IDs, dates, demographic attributes, encounter types, costs, insurance coverage, and procedure details.

**Encounters**

| Table | Field | Description |
|---|---|---|
| encounters | — | Patient encounter data |
| encounters | Id | Primary Key. Unique Identifier of the encounter. |
| encounters | Start | The date and time (ISO8601 UTC Date `yyyy-MM-dd'T'HH:mm'Z'`) the encounter started. |
| encounters | Stop | The date and time (ISO8601 UTC Date `yyyy-MM-dd'T'HH:mm'Z'`) the encounter concluded. |
| encounters | Patient | Foreign key to the Patient. |
| encounters | Organization | Foreign key to the Organization. |
| encounters | Payer | Foreign key to the Payer. |
| encounters | EncounterClass | The class of the encounter, such as ambulatory, emergency, inpatient, wellness, or urgentcare. |
| encounters | Code | Encounter code from SNOMED-CT. |
| encounters | Description | Description of the type of encounter. |
| encounters | Base_Encounter_Cost | The base cost of the encounter, not including any line item costs related to medications, immunizations, procedures, or other services. |
| encounters | Total_Claim_Cost | The total cost of the encounter, including all line items. |
| encounters | Payer_Coverage | The amount of cost covered by the Payer. |
| encounters | ReasonCode | Diagnosis code from SNOMED-CT, only if this encounter targeted a specific condition. |
| encounters | ReasonDescription | Description of the reason code. |

**Patients**

| Table | Field | Description |
|---|---|---|
| patients | — | Patient demographic data. |
| patients | Id | Primary Key. Unique Identifier of the patient. |
| patients | BirthDate | The date (`YYYY-MM-DD`) the patient was born. |
| patients | DeathDate | The date (`YYYY-MM-DD`) the patient died. |
| patients | Prefix | Name prefix, such as Mr., Mrs., Dr., etc. |
| patients | First | First name of the patient. |
| patients | Middle | Middle name of the patient. |
| patients | Last | Last or surname of the patient. |
| patients | Suffix | Name suffix, such as PhD, MD, JD, etc. |
| patients | Maiden | Maiden name of the patient. |
| patients | Marital | Marital Status. M is married, S is single. Currently no support for divorce (D) or widowing (W). |
| patients | Race | Description of the patient's primary race. |
| patients | Ethnicity | Description of the patient's primary ethnicity. |
| patients | Gender | Gender. M is male, F is female. |
| patients | BirthPlace | Name of the town where the patient was born. |
| patients | Address | Patient's street address without commas or newlines. |
| patients | City | Patient's address city. |
| patients | State | Patient's address state. |
| patients | County | Patient's address county. |
| patients | FIPS County Code | Patient's FIPS county code. |
| patients | Zip | Patient's zip code. |
| patients | Lat | Latitude of patient's address. |
| patients | Lon | Longitude of patient's address. |

**Payers**

| Table | Field | Description |
|---|---|---|
| payers | — | Insurance payer data. |
| payers | Id | Primary key of the Payer (e.g. Insurance). |
| payers | Name | Name of the Payer. |
| payers | Address | Payer's street address without commas or newlines. |
| payers | City | Street address city. |
| payers | State_Headquartered | Street address state abbreviation. |
| payers | Zip | Street address zip or postal code. |
| payers | Phone | Payer's phone number. |

**Procedures**

| Table | Field | Description |
|---|---|---|
| procedures | — | Patient procedure data including surgeries. |
| procedures | Start | The date and time (ISO8601 UTC Date `yyyy-MM-dd'T'HH:mm'Z'`) the procedure was performed. |
| procedures | Stop | The date and time (ISO8601 UTC Date `yyyy-MM-dd'T'HH:mm'Z'`) the procedure was completed, if applicable. |
| procedures | Patient | Foreign key to the Patient. |
| procedures | Encounter | Foreign key to the Encounter where the procedure was performed. |
| procedures | Code | Procedure code from SNOMED-CT. |
| procedures | Description | Description of the procedure. |
| procedures | Base_Cost | The line item cost of the procedure. |
| procedures | ReasonCode | Diagnosis code from SNOMED-CT specifying why this procedure was performed. |
| procedures | ReasonDescription | Description of the reason code. |

**Relationships Between Tables**

The tables are connected using primary and foreign keys:
- Patients → Encounters: Patients.Id → Encounters.Patient
- Payers → Encounters: Payers.Id → Encounters.Payer
- Encounters → Procedures: Encounters.Id → Procedures.Encounter
- Patients → Procedures: Patients.Id → Procedures.Patient

These relationships allow patient, encounter, insurance, and procedure information to be analyzed together.

## DATABASE STRUCTURE & RELATIONSHIPS
**Primary Keys**

Primary keys were established on the unique Id columns of the main entity tables:
- Patients.Id
- Payers.Id
- Encounters.Id

These keys uniquely identify patients, payers, and healthcare encounters.

**Foreign Key**

Foreign keys were created to connect related records across the database:
- Encounters.Patient → Patients.Id
- Encounters.Payer → Payers.Id
- Procedures.Encounter → Encounters.Id
- Procedures.Patient → Patients.Id

**Patient–Encounter Relationship**

Each encounter is linked to a patient through Encounters.Patient, which references Patients.Id. This allows patient demographic information to be combined with their healthcare encounters.

**Payer–Encounter Relationship**

Each encounter is linked to an insurance payer through Encounters.Payer, referencing Payers.Id. This enables analysis of healthcare costs and insurance coverage by payer.

**Encounter–Procedure Relationship**

Procedures are linked to the encounters in which they were performed through Procedures.Encounter, referencing Encounters.Id.

**Patient–Procedure Relationship**

Procedures are also directly linked to patients through Procedures.Patient, referencing Patients.Id. This allows procedures to be analyzed at the individual patient level.

SQL Queries:

```SQL
--1. Encounters and Tatients Tables -- Unique ID to the patients
	--Encounter table(PATIENT) -- Foreign key
	--Patients table(id) -- Primary Key

	--Adding primary key to ID column on Patient Table
Alter Table patients
Add constraint PK_id_patients
Primary key (id)

	--Adding Foreign key to PATIENT column on encounter Table
Alter table encounters
Add Constraint FK_Patient_encounters
Foreign key (Patient) References patients (id)

--2. Encounters and payers Tables -- Unique ID to the payer 
	--Ecounters table(PAYER) -- Foreign Key
	--Payers table(id) -- Primary Key

	--Adding primary key to ID column on Payers Table
Alter Table payers
Add Constraint PK_id_payers
Primary Key (id)

	--Adding Foreign Key to PAYER column on ecounters table
Alter Table encounters
Add Constraint FK_Payer_encounter
Foreign Key (Payer) References Payers (id)

--3. Encounter and Procedures Table -- Unique ID to the encounter
	--Encounter table(id) -- Primary Key
	--Procedure table(Encounter) - Foreign Key

	--Adding Primary Key to ID column on Encounter Table
Alter Table Encounters
Add Constraint PK_ID_Encounters
Primary Key (id)
	
	--Adding Foreign Key to Encounter Column on Procedure Table
Alter Table Procedures
Add Constraint FK_Encounter_Procedures
Foreign Key (Encounter) References Encounters (id) 

--4. Pateints and Procedure Table -- Unique ID to the Patient
	--Pateints Table(ID) -- Primary Key
	--Procedures Table(Patient) -- Foreign Key

	--Adding Primary Key to ID column on Patients Table
Alter Table Patients
Add constraint PK_id_Patients
Primary Key (id)
--(Already did this above in No 1)

	--Adding Foreign Key to Patient column on Procedures Table
Alter Table Procedures
Add Constraint FK_Patient_Procedures
Foreign Key (Patient) References Patients (id)
```

Database Diagram:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Data%20Modelling.png)

## DATA QUALITY ASSESSMENT
Before analysis, the dataset was assessed for duplicates, missing values, unreasonable costs, and date inconsistencies.

**Duplicate Record Checks**

Duplicate records were checked in the Patients, Encounters, and Procedures tables. No duplicates were identified.

**Missing Value Checks**

Key demographic, encounter, and procedure fields were checked for missing values, including dates, patient IDs, payer IDs, gender, race, ethnicity, and procedure information. No missing values were identified in the fields tested.

**Outlier Checks**

Cost fields were checked for unreasonable negative values. No negative costs were identified in the Encounters or Procedures tables.

**Cost Validation**

Encounter costs were validated by checking whether Payer_Coverage exceeded Total_Claim_Cost, which could indicate an inconsistent record. No such inconsistencies were identified.

**Date Consistency Checks**

Encounter dates were checked to ensure that the Stop date was not earlier than the Start date. One invalid encounter date was identified and corrected.
Patient death dates were also checked against birth dates, with no invalid death dates identified.

**Encounter After Patient Death**

Encounters occurring after a patient's recorded death date were investigated. 68 potential cases were identified. These records were retained because such occurrences could potentially reflect data or real-world circumstances that could not be conclusively determined from the available data.

**Data Quality Findings**

Overall, the dataset was largely consistent, with no duplicates, missing values in the tested fields, or negative costs identified. One invalid encounter date was corrected, while 68 encounters occurring after recorded death dates were retained for further consideration.

SQL Queries:

```SQL
--Data quality checks:
-- Patients Table
	-- Checking for duplicate values
Select Distinct *
from Patients

	--Ckecking missing demographics
Select 
	Sum (CASE when BIRTHDATE is Null Then 1 Else 0 END) As Missing_BirthDate,
	Sum (CASE when Gender is null then 1 else 0 End) As Missing_Gender,
	Sum (CASE when Race is null then 1 else 0 End) As Missing_Gender,
	Sum (CASE when Ethnicity is null then 1 else 0 End) As Missing_Ethnicity
From Patients

--Encounter Table
	--Checking for duplicate values
Select Distinct *
From Encounters

	--Ckecking missing demographics
Select
	Sum (Case When start is null then 1 else 0 end) As  Missing_start_of_encounter,
	Sum (Case when Stop is null then 1 else 0 end) As Missing_end_of_encounter,
	Sum (Case when Payer is null then 1 else 0 end) As Missing_payer,
	Sum (Case when patient is null then 1 else 0 end) As Missing_patient_ID
From encounters

--Procedures Table
	--Checking for duplicate values
Select Distinct *
From Procedures

	--Ckecking missing demographics
Select 
	Sum (Case when start is null then 1 else 0 end) As Missing_start_of_procedure,
	Sum (Case when stop is null then 1 else 0 end) As Missing_end_of_procedure,
	Sum (Case when code is null then 1 else 0 end) As Missing_procedure_code,
	Sum (Case when description is null then 1 else 0 end) As Missing_description
From Procedures

--OUTLIERS
--Checking for unreasonable values (like negative costs)

--Encounters Table: Cost Outliers
	--For Base_Encounter_Cost
Select
	Count (*) As Negative_Base_Encounter_Cost
From Encounters
Where Base_Encounter_Cost < 0

	--For Total_Claim_Cost
Select 
	Count (*) As Negative_Total_Claim_Cost
From Encounters
Where Total_Claim_Cost < 0

	--Where Payer_Coverage > Total_Claim_Cost (Possible Error)
Select 
	Count (*) As Possible_Error
From Encounters
Where Payer_Coverage > Total_Claim_Cost

--Procedures Table: Cost Outlier
	--For Base_Cost
Select 
	Count (*) As Negative_Base_Cost
From Procedures
Where Base_Cost < 0

--CONSISTENCY CHECKS
--Encounter Start and Stop Logic: Checking where the stop of encounter is lesser than start of encounter
Select 
	Count (*) As Invalid_Date
From Encounters
Where STOP < START

--DeathDate Consistency: Checking for Patients with DeathDate before BirthDate
Select 
	Count (*) As Invalid_DeathDate
From Patients
Where DeathDate is NOT NULL
And DeathDate < BirthDate

--Encounters after Death(Possible Error)
Select 
	Count (*) As Encounter_After_Death
From Encounters
Join Patients
On
Encounters.PATIENT = Patients.Id
Where DeathDate Is NOT NULL
And Encounters.START > patients.DeathDate
```

## DATA CLEANING & ERROR CORRECTION
**Identified Data Issues**

The data quality assessment identified one invalid encounter date where the Stop date occurred before the Start date. In addition, 68 encounters were identified as occurring after recorded patient death dates.

**Invalid Encounter Dates**

The Encounters table was checked for records where Stop < Start. One invalid record was identified and investigated for correction.

**Correction of Encounter Start and Stop Dates**

The identified record was corrected by swapping the Start and Stop values, assuming the dates had been entered in the wrong order.

**Treatment of Encounters After Death**

The 68 encounters occurring after recorded death dates were not removed. They were retained because the available data did not provide enough information to determine whether these represented actual errors or possible real-world/data-recording circumstances.

**Final Data Quality Status**

After correction, the identified invalid encounter date was resolved. Other checked quality issues did not require modification, and the 68 post-death encounters were retained for analysis.

SQL Queries:

```SQL
--FIXING ERRORS AND OUTLIERS
--For the encounters table, checking for the date whose STOP is lesser than START
Select *
From Encounters
Where STOP < START

	--Swap the two dates (asuming it was a mistake)
Update Encounters
Set START = STOP,
	STOP = START
Where STOP < START
```

## FEATURE ENGINEERING
Feature engineering was performed to create additional fields that would make the healthcare data more useful for analysis. Three features were added: patient age at encounter, encounter duration, and out-of-pocket cost.

**Patient Age at Encounter**

A Patients_Age column was added to the Encounters table. Patient age was calculated using the patient's date of birth and the date of the encounter.

SQL Queries:

```SQL
--Patient Age at Encounter (in years)
	--Create a new column - Patient Age
Alter Table Encounters
Add Patients_Age INT
	
	--Populate the the column above
Update Encounters
Set Patients_Age = DATEDIFF(Year, Patients.BirthDate, encounters.start)
From Encounters
Join Patients 
on
encounters.patient = patients.id
```
Purpose: To determine the patient's age at the time of each healthcare encounter and support age-based demographic analysis.

**Encounter Duration**

An Encounter_Duration column was added to capture the length of each encounter. The duration was calculated from the encounter start and stop times.

SQL Queries:

```SQL
--Encounter Duration (In hours)
--Create a new column - Encounter Duration
Alter Table Encounters
Add Encounter_Duration INT

--Update the column above
Update Encounters
Set Encounter_Duration = DATEDIFF(MINUTE, Start, stop)
```
Purpose: To measure how long patients spent in each encounter and support analysis of healthcare utilization.

**Out-of-Pocket Cost**

An Out_Of_Pocket_Cost column was created to calculate the portion of the total claim cost not covered by the payer.

SQL Queries:

```SQL
--Out of pocket cost
--Create a new column
Alter table Encounters
Add Out_Of_Pocket_Cost Float

	--Update the column above
Update Encounters
Set Out_Of_Pocket_Cost = Round(TOTAL_CLAIM_COST - PAYER_COVERAGE,2)
```

## PATIENT DEMOGRAPHIC ANALYSIS
This section examines the demographic characteristics of patients in the dataset, including age, gender, race, ethnicity, marital status, and geographic distribution. The analysis helps provide an understanding of the population served by the hospital.

**Patient Age Groups**

Patients were grouped into four age categories to understand the distribution of the patient population.

SQL Queries:

```SQL
--Patient Demographic Analysis
	--Age Group Categorization
Select 
	CASE 
		When DATEDIFF(Year, BirthDate, '2022-12-31') < 18 Then '0-17'
		When DATEDIFF(Year, BirthDate, '2022-12-31') Between 18 and 34 Then '18-34'
		When DATEDIFF(Year, BirthDate, '2022-12-31') Between 35 and 49 Then '35-49'
		When DATEDIFF(Year, BirthDate, '2022-12-31') Between 50 and 64 Then '50-64'
		Else '65+'
		End As Age_Group,
	Count(*) As PatientCount
From Patients
Where BirthDate is Not Null
Group by 
	CASE 
		When DATEDIFF(Year, BirthDate, '2022-12-31') < 18 Then '0-17'
		When DATEDIFF(Year, BirthDate, '2022-12-31') Between 18 and 34 Then '18-34'
		When DATEDIFF(Year, BirthDate, '2022-12-31') Between 35 and 49 Then '35-49'
		When DATEDIFF(Year, BirthDate, '2022-12-31') Between 50 and 64 Then '50-64'
		Else '65+'
		End
Order By Age_Group
```

Result: 

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Age%20Group%20Categorization.png)

Insight: The 65+ age group dominates the patient population, with 587 patients. This indicates that the dataset is heavily concentrated among older adults, while patients aged 18–34 represent the smallest group.

**Age Distribution Over Time**

Average, youngest, and oldest patient ages were analyzed by year to understand how the age profile changed over the study period.

SQL Queries:

```SQL
	--Age Distribution by year
	--For yearly comparison (Age at encounter)
Select 
	Year(encounters.start) As Year,
	AVG(DATEDIFF(Year, patients.BirthDate, encounters.start)) As AvgAge,
	MIN(DATEDIFF(Year, patients.BirthDate, encounters.start)) As Youngest,
	MAX(DATEDIFF(Year, patients.BirthDate, encounters.start)) As Oldest
From encounters
Join patients
On
encounters.patient = patients.ID
Group by Year(encounters.start)
Order by Year
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Age%20Distribution%20by%20Year.png)
 
Insight: The average patient age generally increased over the study period, reaching 79 years in 2022. The youngest age also increased from 20 to 31, while the oldest age increased from 89 to 99.

**Gender Distribution**

The patient population was analyzed by gender.

SQL Queries:

```SQL
	--Gender Distribution
SELECT 
    Gender,
    COUNT(*) AS PatientCount,
    Round((COUNT(*) * 100.0 / (SELECT COUNT(*) FROM Patients WHERE Gender IS NOT NULL)),2) AS Percentage
FROM Patients
WHERE Gender IS NOT NULL
GROUP BY Gender;
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Gender%20Distribution.png)

Insight: The gender distribution is relatively balanced, with males representing 50.72% and females 49.28% of the analyzed patients.

**Race Distribution**

Patient counts were analyzed across the available racial categories.

SQL Queries:

```SQL
	--Race Breakdown
SELECT 
    Race,
    COUNT(*) AS PatientCount,
    Round((COUNT(*) * 100.0 / (SELECT COUNT(*) FROM Patients WHERE Race IS NOT NULL)),2) AS Percentage
FROM Patients
WHERE Race IS NOT NULL
GROUP BY Race;
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Race%20Breakdown.png)
 
Insight: White patients account for the largest proportion, representing 69.82% of the analyzed population. Black patients account for 16.74%, while Asian patients represent 9.34%.

**Ethnicity Distribution**

Patient ethnicity was analyzed to understand the composition of the population.

SQL Queries:

```SQL
	--Ethnicity Breakdown
SELECT 
    Ethnicity,
    COUNT(*) AS PatientCount,
    Round((COUNT(*) * 100.0 / (SELECT COUNT(*) FROM Patients WHERE Ethnicity IS NOT NULL)),2) AS Percentage
FROM Patients
WHERE Ethnicity IS NOT NULL
GROUP BY Ethnicity;
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Ethnicity%20Breakdown.png)

Insight: Non-Hispanic patients represent the majority of the population at 80.39%, while Hispanic patients account for 19.61%.

**Marital Status Analysis**

Patients were grouped by marital status.

SQL Queries:

```SQL
	--Marital Status Analysis
Select
	Marital,
	CASE	
		When Marital = 'M' Then 'Married'
		When Marital = 'S' Then 'Single'
		Else 'Unknown'
		End As Marital_Status,
	Count(*) As PatientCount,
	Round(100.0 * Count(*) / (Select Count(*) From Patients),2) As Percentage
From Patients
Group By Marital
````

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Marital%20Status%20Analysis.png)
 
Insight: The majority of patients are married (80.49%), while 19.40% are single. Only one patient has an unknown marital status.

**Geographic Distribution by County**

Patient distribution was analyzed by county to identify where the patient population is concentrated.

SQL Queries:

```SQL
	--By County
Select
	County,
	Count(*) As PatientCount
From Patients
Where state is Not Null
Group by County
Order by PatientCount DESC
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Goegraphical%20Distribution%20By%20County.png)

Insight: Suffolk County has the highest concentration of patients, with 644 patients, followed by Norfolk and Middlesex Counties. Essex County has the smallest representation, with only one patient.

## MEDICAL ENCOUNTER ANALYSIS
This section analyzes healthcare encounters to understand encounter volume, service types, duration, diagnoses, readmissions, and associated costs.

**Encounter Volume Over Time**

The number of healthcare encounters was analyzed by year to identify changes in healthcare utilization over the study period.

SQL Queries:

```SQL
--Encounter Trend over time
Select 
	Year(Start) As Year,
	Count (*) As Encounter_Count
From Encounters
Group by Year(Start)
Order by Year
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Encounter%20Trend%20Over%20Time.png)

Insight: Encounter volume increased from 2011 and reached its highest level in 2014 with 3,885 encounters. Volume remained relatively stable from 2015–2019, increased again in 2020–2021, and dropped substantially in 2022.

**Monthly Encounter Trends**

Encounters were grouped by month to identify seasonal patterns in healthcare utilization.

SQL Queries:

```SQL
--Encounter Trend by Month
Select
	Month(Start) As Month,
	Count (*) As Encounter_Count
From Encounters 
Group by 
	Month(Start)
Order by
	Month
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Encounter%20Trend%20by%20Month.png)

Insight: February recorded the highest number of encounters (3,023), while October recorded the lowest (2,089). The results show some variation in monthly healthcare utilization.

**Encounter Types**

Encounters were analyzed by encounter class to understand the distribution of healthcare services.

SQL Queries:

```SQL
--Encounter Types (Encounter Class)
Select 
	EncounterClass,
	Count (*) As Total_Encounter,
	Round((100 * Count (*) / (Select 
							Count (*)
							From Encounters)), 2) As Perc_Contr
From Encounters
Group by 
	EncounterClass
Order by 
	Total_Encounter
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Encounter%20Class%20Distribution.png) 

Insight: Ambulatory encounters account for the largest share (44%), followed by outpatient encounters at 22%. Inpatient encounters represent the smallest proportion among the listed encounter classes at 4%.

**Average Encounter Duration**

Average encounter duration was analyzed across encounter classes.

SQL Queries:

```SQL
--Average Length of Encounter (Duration)
Select 
	EncounterClass,
	AVG(DATEDIFF(Hour, Start, Stop)) As Avg_Duration_Hours,
	AVG(DATEDIFF(Day, Start, Stop)) As Avg_Duration_Days
From Encounters
Group By
	EncounterClass
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Average%20Length%20of%20Encounter.png)

Insight: Inpatient encounters have the longest average duration at 36, while wellness and urgent care encounters have an average duration of 0 based on the calculated values.

**Top Diagnoses by Frequency and Cost**

Diagnoses were analyzed using encounter frequency, average cost, and total cost to identify conditions with significant healthcare activity or financial impact.

SQL Queries:

```SQL
--Top Diagonises by frequency and cost
Select Top 10
	ReasonDescription,
	Count(*) As Encounter_Count,
	Round(AVG(Total_claim_cost),0) As Avg_Cost,
	Round(Sum(Total_Claim_Cost), 0) As Total_cost
From Encounters
Where
	ReasonDescription Is Not Null
Group by ReasonDescription
Order by
	Total_cost Desc
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Top%20Diagonises%20by%20Frequency%20and%20Cost.png)

Insight: Chronic congestive heart failure has the highest encounter frequency among the listed diagnoses, with 1,738 encounters. Normal pregnancy has the highest total cost at approximately $20.9 million. Sepsis has the highest average cost per encounter at approximately $161,113, despite only four recorded encounters.

**Patient Readmission Analysis**

A self-join analysis was used to identify patients with multiple healthcare encounters and examine the frequency of repeated encounters.

SQL Queries:

```SQL
--Top 20 Readmission Rates (Self Join)
Select Top 20
	e1.patient,
	Count (*) As Readmission_Count
From Encounters e1
Join Encounters e2
On
e1.Patient = e2.patient
And e1.ID <> e2.ID
And DATEDIFF(Day, e1.stop, e2.start) Between 1 and 30
Group by e1.patient
Having Count(*) > 1
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Top%2020%20Readmission%20Rates.png)

Insight: The results show substantial variation in repeated healthcare encounters across patients. One patient recorded 824 repeated encounters, while several patients recorded only two or a few encounters. Patients with unusually high encounter counts may warrant further investigation into chronic conditions, recurring care, or data patterns.

**Encounter Cost Analysis**

Encounter costs were analyzed using base encounter cost, total claim cost, and payer coverage.

Overall Cost Result:

| Metric | Value |
|---|---:|
| Average Base Cost | $116 |
| Average Claim Cost | $3,640 |
| Total Base Cost | $3,240,421 |
| Total Claim Cost | $101,514,375 |

SQL Queries:

```SQL
	--a. Average Base Cost
Select 
	Concat('$', Format(AVG(Base_Encounter_Cost), 'N0')) As Avg_Base_Cost		
From Encounters

	--b. Average Claim Cost
Select
	Concat('$', Format(AVG(Total_Claim_Cost), 'N0')) As Avg_Claim_Cost  
From Encounters

	--c. Total Base Cost
Select 
	Concat('$', Format(Sum(Base_Encounter_Cost), 'N0')) As Total_Base_Cost
From Encounters

	--d. Total Claim Cost
Select
	Concat('$', Format(Sum(Total_Claim_Cost), 'N0')) As total_Claim_Cost 
From Encounters
```

Payer Coverage Result:

SQL Queries:

```SQL
	--e. Coverage percentage by payer
Select
	Payers.Name As Payer_Name,
	Round(AVG(100 * Payer_Coverage / Nullif (Total_Claim_cost, 0)), 2) As Avg_Coverage_Percent
From Encounters
Join Payers
On
Encounters.Payer = Payers.ID
Group by Payers.Name
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Coverage%20Percentage%20by%20Payer.png)

Insight: The dataset contains approximately $101.5 million in total claim costs, compared with approximately $3.24 million in total base costs. Among the listed payers, Medicaid has the highest average coverage at 74.55%, followed by Medicare at 62.95%. Several payers show very low or zero average coverage in the calculated results.

## INSURANCE & FINANCIAL ANALYSIS
This section examines healthcare costs, payer contributions, insurance coverage, and patient out-of-pocket expenses to understand the financial aspects of healthcare utilization.

**Total Claim Cost by Payer and Average Claim Cost by Payer**

Total claim costs, encounter volumes, and average claim costs were analyzed by payer.

```SQL
SQL Queries:
--Total Claim cost by payer (Who pays the most)
Select
	Payers.Name As Payer_Name,
	Round(Sum(encounters.Total_Claim_Cost),2) As Total_Claim_Cost,
	Count(encounters.Id) As Encounter_Count,
	Round(Avg(encounters.Total_Claim_Cost), 2) As Avg_Claim_Per_Encounter
From Encounters
Join Payers
On
Encounters.payer = Payers.Id
Group by Payers.Name
Order by Total_Claim_Cost
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Total%20Claim%20cost%20by%20payer.png)

Insight: NO_INSURANCE accounts for the largest total claim cost at approximately $49.3 million, while Medicare has the highest encounter volume with 11,371 encounters. Medicaid has the highest average claim cost per encounter at approximately $6,205.

**Payer Coverage Ratio and Average Out-of-Pocket Cost by Payer**

Average payer coverage and average patient out-of-pocket costs were analyzed to understand the level of financial protection provided by each payer.

SQL Queries:

```SQL
--This measures how much of the total claim cost is covered by insurance versus paid by patients
Select
	Payers.Name,
	Round(AVG(100 * Encounters.payer_coverage / Nullif (Encounters.Total_Claim_Cost, 0)), 2) As Avg_Coverage_Percentage,
	Round(AVG(Encounters.Total_Claim_Cost - Encounters.Payer_Coverage), 2) As Avg_Out_Of_Pocket_Cost
From Encounters
Join Payers
On
Encounters.Payer = Payers.Id
Group by Payers.Name
Order by Avg_Coverage_Percentage DESC
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Payer%20Coverage%20Ratio.png)

Insight: Medicaid has the highest average coverage at 74.55%, while Medicare provides 62.95% average coverage. NO_INSURANCE has no payer coverage and the highest average out-of-pocket cost at $5,593.20.

**Out-of-Pocket Cost by Age Group**

Out-of-pocket costs, average coverage, and encounter volume were analyzed across age groups.

SQL Queries:

```SQL
	--Examining whether certain groups (by age or gender) pay more out of pocket than others
	--a. By Age Group
Select
	CASE
		When DATEDIFF(Year, Patients.BirthDate, encounters.Start) < 18 Then '0-17'
		When DATEDIFF(Year, Patients.BirthDate, encounters.Start) Between 18 and 34 Then '18-34'
		When DATEDIFF(Year, Patients.BirthDate, encounters.Start) Between 34 and 54 Then '34-54'
		When DATEDIFF(Year, Patients.BirthDate, encounters.Start) Between 54 and 74 Then '54-74'
		Else '75+'
	End As Age_Group,
	Round(AVG(Encounters.Total_Claim_Cost - Encounters.Payer_Coverage), 2) As Avg_Out_Of_Pocket_Cost,
	Round(AVG(100 * Encounters.Payer_Coverage / Nullif (Encounters.Total_Claim_Cost, 0)), 2) As Avg_Coverage_Percentage,
	Count (*) As Encounter_Count
From Encounters
Join Patients
On 
Encounters.Patient = Patients.Id
Group by 
	CASE
		When DATEDIFF(Year, Patients.BirthDate, encounters.Start) < 18 Then '0-17'
		When DATEDIFF(Year, Patients.BirthDate, encounters.Start) Between 18 and 34 Then '18-34'
		When DATEDIFF(Year, Patients.BirthDate, encounters.Start) Between 34 and 54 Then '34-54'
		When DATEDIFF(Year, Patients.BirthDate, encounters.Start) Between 54 and 74 Then '54-74'
		Else '75+'
	End
Order By 
	Age_Group
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Out%20of%20pocket%20cost%20burden%20by%20Age%20Group.png)

Insight: The 18–34 age group has the highest average out-of-pocket cost ($4,868.63) and relatively low average coverage of 21.29%. The 75+ group has the highest encounter volume and the highest average coverage at 39.38%.

**Out-of-Pocket Cost by Gender**

Average out-of-pocket costs and coverage were compared by gender.

SQL Queries:

```SQL
--b. By Gender
Select 
	Patients.Gender,
	Round(AVG(Encounters.Total_Claim_Cost - Encounters.Payer_Coverage), 2) As Avg_Out_of_Pocket_Cost,
	Round(AVG(100 * Encounters.Payer_Coverage / Nullif (Encounters.Total_Claim_Cost, 0)), 2) As Avg_Coverage_Percentage,
	Count (*) As Encounter_Cost
From Encounters
Join Patients
On
Encounters.patient = Patients.Id
Where Patients.Gender is Not Null
Group by Patients.Gender
Order by Avg_Out_of_Pocket_Cost
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Out%20of%20pocket%20cost%20burden%20by%20Gender.png)

Insight: Male patients have a higher average out-of-pocket cost of $2,955.44, compared with $2,150.47 for female patients. Average coverage is slightly higher for male encounters.

**Uncovered Cost Trends Over Time**

Average out-of-pocket costs and average payer coverage were analyzed by year to identify changes in patients' financial responsibility over time.

SQL Queries:

```SQL
	--Checking whether patient are paying more out of pocket as time progress
Select 
	Year(Encounters.Start) As Year,
	Round(AVG(Encounters.Total_Claim_Cost - Encounters.Payer_Coverage), 2) As Avg_Out_of_Pocket_Cost,
	Round(AVG(100 * Encounters.Payer_Coverage / Nullif (Encounters.Total_Claim_Cost, 0)), 2) Avg_Coverage_Percentage
From Encounters
Where Encounters.Total_Claim_Cost Is Not Null
Group by Year(Encounters.Start)
Order by Year
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Uncovered%20Cost%20Trends%20Over%20Time.png)

Insight: Average out-of-pocket costs fluctuated throughout the study period. The highest average was recorded in 2012 at $2,936.83, while the lowest was recorded in 2011 at $1,733.38. Average payer coverage ranged from 28.86% to 36.04% across the years.


## MEDICAL PROCEDURE ANALYSIS
This section analyzes medical procedures to identify the most frequently performed procedures, high-cost procedures, trends over time, associated conditions, and procedure distribution across encounter types.

**Most Common Procedures**

The most frequently performed procedures were identified by counting the number of times each procedure appeared in the dataset.

SQL Queries:

```SQL
--Top 10 Most common procedures by frequency
Select Top 10
	Description As Procedure_Name,
	Count(*) As Procedure_Count
From Procedures
Group by Description
Order by Procedure_Count DESC
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Top%2010%20Most%20common%20procedures%20by%20frequency.png)

Insight: Assessment of health and social care needs was the most frequently recorded procedure, with 4,596 occurrences, followed by hospice care with 4,098 occurrences. Several of the most common procedures involve screening, assessment, and ongoing patient care.

**Most Expensive Procedures**

Procedures were analyzed based on their average procedure cost to identify procedures with the highest average financial cost.

SQL Queries:

```SQL
--Top 10 Most Expensive Procedures (Average Cost)
Select Top 10
	Description As Procedure_Name,
	AVG(Base_Cost) As Avg_Procedure_Cost,
	Count(*) As Procedure_Count
From Procedures
Where Base_Cost is Not Null
Group by Description
Having Count(*) > 5 -- Filtering out very rare procedures for stability
Order by Avg_Procedure_Cost DESC
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Top%2010%20Most%20Expensive%20Procedures.png)

Insight: Coronary artery bypass grafting has the highest average procedure cost at approximately $47,085, followed by hemodialysis at $29,299. Some high-cost procedures have relatively low procedure counts, while electrical cardioversion has a much higher volume of 1,383 procedures.

**Procedure Trends Over Time**

The number of procedures and average procedure count were analyzed by year to identify changes in procedure activity.

SQL Queries:

```SQL
--Procedure Trend Over Time
Select 
	Year(Start) As Year,
	Count(*) As Total_Procedure,
	Avg(Base_Cost) As Avg_procedure_cost
From Procedures
Group by Year(Start)
Order by Year
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Procedure%20Trend%20Over%20Time.png)

Insight: Total procedure volume reached its highest level in 2014 with 6,292 procedures. Procedure activity generally remained between approximately 3,800 and 5,000 procedures annually from 2015–2021 before dropping substantially in 2022.

**Procedure Reasons and Associated Conditions**

Procedures were grouped by their associated conditions to identify the medical reasons most frequently linked to procedures and their average base costs.

SQL Queries:

```SQL
--Top 10  Procedure Reasons (Linking to Conditions)
Select Top 10
	ReasonDescription,
	Count(*) As Procedure_Count,
	AVG(Base_Cost) As Avg_Base_Cost
From Procedures
Where ReasonDescription is Not Null
Group by ReasonDescription
Order by Procedure_Count Desc
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Top%2010%20%20Procedure%20Reasons.png)

Insight: Normal pregnancy is associated with the highest procedure volume at 5,718 procedures. Atrial fibrillation has the highest average base cost among the listed conditions at approximately $22,476.

**Procedures by Encounter Type**

Procedure volume and average base cost were analyzed across different encounter classes.

SQL Queries:

```SQL
--5. Procedures Linked to Encounter Types
Select 
	EncounterClass,
	Count(*) As Total_Procedures,
	AVG(Base_Cost) As Avg_base_cost
From Procedures
Join Encounters
On
Procedures.Encounter = Encounters.ID
Group by EncounterClass
Order by Avg_base_cost Desc
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Procedures%20Linked%20to%20Encounter%20Types.png)

Insight: Ambulatory encounters account for the highest procedure volume, with 17,822 procedures, followed by outpatient encounters with 14,958. Urgent care has the highest average base cost at approximately $21,752 despite having a relatively low procedure volume.

**Average Procedures per Encounter Class**

The average number of procedures performed per encounter was analyzed by encounter class.

SQL Queries:

```SQL
--Average Number of procedures per Encounter Class
Select
	EncounterClass,
	Count(*) / Count(Distinct Encounters.ID) As Avg_procedures_per_encounter
From Procedures
Join Encounters
On
Procedures.Encounter = Encounters.ID
Group by EncounterClass
Order by Avg_procedures_per_encounter Desc
```

Result:

![](https://github.com/Oluwaseun2024-ctrl/Hospital-Records-Analysis-With-SQL/blob/main/Average%20Number%20of%20procedures%20per%20Encounter%20Class.png)

Insight: Wellness encounters have the highest average number of procedures per encounter at 5, followed by outpatient encounters at 4. Urgent care and emergency encounters average one procedure per encounter.


## KEY FINDINGS & INSIGHTS
The analysis identified several important patterns across patient demographics, healthcare encounters, financial costs, insurance coverage, procedures, and readmissions.

**Patient Demographic Insights**
- The patient population is heavily concentrated among older adults, with 587 patients aged 65+.
- The average patient age generally increased over the study period, reaching 79 years in 2022.
- Gender distribution was relatively balanced, with males representing 50.72% and females 49.28%.
- White patients represented the largest racial group at 69.82%.
- 80.39% of patients were non-Hispanic.
- Married patients represented 80.49% of the population.
- Suffolk County had the highest patient concentration, with 644 patients.

**Encounter Insights**
- Annual encounter volume reached its highest point in 2014 with 3,885 encounters and increased again to 3,530 in 2021.
- February recorded the highest monthly encounter volume with 3,023 encounters.
- Ambulatory encounters represented the largest encounter category at 44%, followed by outpatient encounters at 22%.
- Inpatient encounters had the longest average duration at 36 minutes based on the engineered duration field.
- Chronic congestive heart failure had the highest encounter frequency among the listed diagnoses, with 1,738 encounters.
- Normal pregnancy generated the highest total cost among the listed diagnoses, at approximately $20.9 million.

**Financial & Insurance Insights**
- Total claim costs amounted to approximately $101.5 million.
- NO_INSURANCE accounted for the largest total claim cost at approximately $49.3 million.
- Medicare had the highest encounter volume among the payers, with 11,371 encounters.
- Medicaid had the highest average payer coverage at 74.55%, followed by Medicare at 62.95%.
- Encounters without insurance had the highest average out-of-pocket cost at approximately $5,593.
- The 18–34 age group had the highest average out-of-pocket cost at approximately $4,869.
- Male encounters had a higher average out-of-pocket cost than female encounters.

**Procedure Insights**
- Assessment of health and social care needs was the most frequently recorded procedure, with 4,596 occurrences.
- Coronary artery bypass grafting had the highest average procedure cost at approximately $47,085.
- Total procedure volume was highest in 2014 with 6,292 procedures.
- Normal pregnancy was associated with the highest procedure volume, with 5,718 procedures.
- Atrial fibrillation had the highest average base cost among the listed procedure-associated conditions at approximately $22,476.
- Ambulatory encounters accounted for the highest procedure volume with 17,822 procedures.
- Wellness encounters had the highest average number of procedures per encounter at 5.

**Readmission Insights**
- The readmission analysis showed substantial variation in the number of repeated encounters across patients.
- Several patients had relatively few repeated encounters, while some patients had substantially higher encounter counts.
- The highest recorded patient readmission count in the analyzed results was 824.
- Patients with unusually high numbers of repeated encounters represent potential areas for further investigation into recurring healthcare utilization and underlying conditions.

## RECOMMENDATIONS
Based on the findings from the analysis, the following recommendations can support healthcare management, cost management, patient care, and future analytical work.

**Recommendations for Healthcare Management**
- Monitor healthcare utilization trends, particularly changes in annual and monthly encounter volumes.
- Pay attention to the high concentration of older patients when planning healthcare resources and services.
- Monitor high-volume encounter categories, particularly ambulatory and outpatient services.
- Investigate patients with unusually high numbers of repeated encounters to better understand the factors contributing to frequent healthcare utilization.
- Use demographic and geographic patterns to support resource allocation and service planning.

**Recommendations for Cost Management**
- Investigate the high claim costs associated with uninsured encounters to understand the financial impact of limited insurance coverage.
- Monitor diagnoses and procedures with high average or total costs, such as sepsis, coronary artery bypass grafting, and hemodialysis.
- Analyze payer coverage patterns to identify areas of high patient financial responsibility.
- Monitor out-of-pocket costs across age groups and genders to identify differences in patient financial burden.
- Track uncovered costs over time to identify significant changes in healthcare affordability.

**Recommendations for Patient Care**
- Use the demographic profile of patients to support appropriate planning for patient services, particularly for the large 65+ population.
- Monitor frequently occurring conditions such as chronic congestive heart failure and hyperlipidemia.
- Pay attention to patients with high numbers of repeated encounters, as they may require further assessment or coordinated care.
- Continue monitoring preventive and assessment procedures, including depression screening, substance-use assessment, and medication reconciliation.
- Use procedure and encounter patterns to identify opportunities for improving continuity and coordination of care.

**Recommendations for Further Analysis**
- Investigate the causes of extremely high readmission counts among individual patients.
- Perform deeper analysis of the relationship between diagnoses, procedures, encounter types, and healthcare costs.
- Analyze patient-level healthcare utilization to identify high-utilization patient groups.
- Examine insurance coverage and out-of-pocket costs at the patient level.
- Conduct time-series analysis to investigate changes in healthcare utilization and costs over the study period.
- Develop dashboards to monitor key healthcare, financial, and utilization indicators.
- Extend the analysis with statistical or machine learning techniques to identify factors associated with high healthcare costs or repeated encounters.

## CONCLUSION
This project demonstrated how SQL can be used to transform healthcare data into meaningful insights for healthcare and financial analysis. Using synthetic patient data from Massachusetts General Hospital covering 2011–2022, the analysis examined patient demographics, healthcare encounters, diagnoses, insurance coverage, costs, procedures, and readmissions.

The project also demonstrated practical SQL skills including database relationships, data quality assessment, error correction, feature engineering aggregation, joins, and analytical querying.

The findings revealed a patient population largely concentrated among older adults, significant variation in healthcare utilization and encounter types, substantial differences in insurance coverage and out-of-pocket costs, and notable variation in procedure volume and cost.

Overall, the analysis provides a structured view of patient characteristics, healthcare utilization, and financial patterns within the dataset. It also demonstrates how SQL-based healthcare analytics can support data-driven decision-making and provide a foundation for deeper statistical, business intelligence, or machine learning analysis.
