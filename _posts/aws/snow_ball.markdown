## Snow Ball

### What is Snow Ball (big big disk)
Petabyte-scale data transport solution move data `into and out of` AWS S3.

* Import to S3
* Export from S3


## Storage Gateway

on-premises software


```
virtual / physical types
    --------
    |  your data center     |  |storage gateway| ----sync data to AWS ---> 
    --------
```

#### Types of Storage Gateway

1. File Gateway (NFS & SMB)
    for flat fiels, directly stored on S3.
connect your on-premise application to S3

2. Volume Gateway 
   * Stored Volumes
    entire dataset is stored in data center and async backup to S3
   * Cached Volumes
    entire dataset is on S3, only cache on site
3. Tape Gateway
backup

## Athena Vs. Macie

### Athena
Interactive query service which enables you to analyse
and query data located in S3 using standard SQl.

serverless
commonly analyse logs data in S3

### Macie
Security service (using Machine Learning & NLP) to classify 
and protect sensitive data in S3(PII)

* uses AI identify PII data in S3
* could also be used in CloudTrail logs
* including Dashboards & Reports & Alerting
* great for PCI-DSS (??????) & preventing ID theft
