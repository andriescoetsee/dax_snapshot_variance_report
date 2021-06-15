## Welcome to Dax Concept: Snapshot & Variance Report

![](/images/Dataset Illustration.png)

```Python
Base Claims Paid Value (local) = 
IF(
---- only calculate for values when we have data
    ISBLANK([Sum Claims Paid Value (local)]),  
    BLANK(),
    CALCULATE(    
            [Claims Paid Value (local)],
            ALL('Submission Date')
    )
)
```

Did this work
