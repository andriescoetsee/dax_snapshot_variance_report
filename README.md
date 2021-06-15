# Dax Concept: Snapshot & Variance Report

## Problem Statement

Each file contains a 1:m relationship between the **Processing Month** and the **Reporting Month**

For example processing month March includes reporting months January, February and March. 

**Business Requirement** 
* For each processing month business wants to report only on the latest reporting month as a snapshot that remains fixed for all subsequent reporting.
* Furthermore, business wants to do a variance report on how each reporting month differs across processing months

### Dataset overview
![](/images/Dataset Illustration.png)


### Step 1: Create smallest building block

```
Sum Revenue Amount = 
SUM(
    'Sales per Product'[Revenue Amount]
)
```
---

### Step 2: Now filter to identify snapshot month

```
Snapshot Revenue Amount = 
CALCULATE( 
        [Sum Revenue Amount],
        FILTER(
            'Sales per Product',
            ---- this is where Processing Month = Reporting Month
            'Sales per Product'[IsSnapshot] = TRUE  
        )
)
```
---

### Step 3: Prepare base value for % Diff calculation

```
Base Revenue Amount = 
IF(
    ---- only calculate for values when we have data
    ISBLANK([Sum Revenue Amount]),  
    BLANK(),
    CALCULATE(    
            [Snapshot Revenue Amount],
            ALL('Processing Date Dim')
    )
)
```
---

### Step 4: Calculate % difference compared to the Snapshot

```
% Diff Snapshot Revenue Amount = 
IF(
    ISBLANK([Sum Revenue Amount]),
    BLANK(),
    DIVIDE(
        [Sum Revenue Amount] - [Base Revenue Amount],
        [Base Revenue Amount]
    )
)
```
---

### Step 5: Calculate Previous processing month in prep for next step

```
Revenue Amount (prev processing month) = 
IF(
    ISBLANK([Base Revenue Amount]),
    BLANK(),
    CALCULATE(
        [Sum Revenue Amount],
        DATEADD(
            'Processing Date Dim'[Date],
            -1,
            MONTH
        )   
    )
)
```
---

### Step 6: Calculate % Diff Previous processing month 

```
% Diff prev processing month = 
IF(
    ISBLANK([Sum Revenue Amount]),
    BLANK(),
    DIVIDE(
        [Sum Revenue Amount] - [Revenue Amount (prev processing month)],
        [Revenue Amount (prev processing month)]
    )
)
```
---
