
# Setting Up Data Pipeline with Google Cloud Dataflow, BigQuery and Pub/Sub (Stream Data)

This repository demonstrates how to transform data from a Pub/Sub using Google Cloud Dataflow and store it in BigQuery.

## Prerequisites

- Google Cloud SDK installed
- Google Cloud project setup
- Google Cloud Storage bucket created
- Python installed

## Steps

### 1. Create a Bucket to store output and temporary data

1. Create a new bucket in Google Cloud Storage.

### 2. Create a Pub/Sub topic to send messages

### 3. Create a bigquery dataset and a table with schema

```sh
dataset: dataset_demo
   Table: review_table
   Schema:
        "url:STRING",
        "num_reviews:INTEGER",
        "score:FLOAT64",
        "first_date:TIMESTAMP",
        "last_date:TIMESTAMP"
```

### 4. Open Cloudshell and Set Up Python Virtual Environment, Install Apache Beam SDK

Install the `virtualenv` module, create a virtual environment, and then activate it:

```sh
pip3 install virtualenv
python3 -m virtualenv env
source env/bin/activate

#install Apache Beam SDK
pip3 install apache-beam[gcp] 
```

### 5. Upload the python script and run the pipeline in the cloudshell
```sh
python3 stream-review.py \
--input_subscription projects/techlanders-internal/subscriptions/review_topic-sub \
--output_table techlanders-internal:dataset_demo.review_table \
--project techlanders-internal \
--region us-central1 \
--zone=us-central1-b \
--window_size=2 \
--num_shards=2 \
--temp_location gs://sandeep-apache/temp \
--runner DataflowRunner \
--max_num_workers=4
```

### 6. Go to pub/sub and publish some messages.
```sh
{
  "url": "http://example.com",
  "review": "positive"
}
```




