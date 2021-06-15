## Welcome to Dax Concept: Snapshot & Variance Report

![](/images/Dataset Illustration.png)

```
Base Claims Paid Value (local) = 
IF(
    ISBLANK([Sum Claims Paid Value (local)]),  ---- only calculate for values when we have data
    BLANK(),
    CALCULATE(    
            [Claims Paid Value (local)],
            ALL('Submission Date')
    )
)
```

Did this work
