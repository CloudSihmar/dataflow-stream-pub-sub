Setting Up Data Pipeline with Google Cloud Dataflow and BigQuery
This guide demonstrates how to set up a data pipeline using Google Cloud Dataflow to process data from Pub/Sub and store it in BigQuery.

Steps
1. Create Necessary Resources
Create a Bucket for Temporary Data

Create a Google Cloud Storage bucket to store temporary data during processing.

Create a Pub/Sub Topic and Subscription

Set up a Pub/Sub topic and subscription to handle incoming messages.

Create a BigQuery Dataset and Table

Create a dataset named dataset_demo and a table named review_table in BigQuery with the following schema:

plaintext
Copy code
"url:STRING", "num_reviews:INTEGER", "score:FLOAT64", "first_date:TIMESTAMP", "last_date:TIMESTAMP"
2. Set Up Python Environment
Install the virtualenv module, create a virtual environment, and activate it:

sh
Copy code
pip3 install virtualenv
python3 -m virtualenv env
source env/bin/activate
3. Install Apache Beam SDK
Install Apache Beam SDK with Google Cloud Platform support:

sh
Copy code
pip3 install apache-beam[gcp]
4. Upload and Run the Pipeline
Upload your Python script (stream-review.py) to Cloud Shell and run the pipeline with the following command:

sh
Copy code
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
Replace placeholders (projects/techlanders-internal/subscriptions/review_topic-sub, techlanders-internal:dataset_demo.review_table, etc.) with your actual Pub/Sub subscription and BigQuery table details.

5. Publish Messages to Pub/Sub
Go to Pub/Sub and publish messages in JSON format, such as:

json
Copy code
{ "url": "http://example.com", "review": "positive" }
This data will be processed by the pipeline.

---




1. Create a bucket to store the outout or temporary data
2. Create a Pub/Sub to send messages.
3. Create a bigquery dataset and a table with a schema
   dataset: dataset_demo
   Table: review_table
   Schema:
        "url:STRING",
        "num_reviews:INTEGER",
        "score:FLOAT64",
        "first_date:TIMESTAMP",
        "last_date:TIMESTAMP",


4. Install the virtualenv module, create a virtual environment, and then activate it:

pip3 install virtualenv
python3 -m virtualenv env
source env/bin/activate

5. Install Apache Beam SDK
pip3 install apache-beam[gcp]

6. Upload the python script and run the pipeline in the cloudshell

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

7. Go to pub/sub and publish some messages.

{
  "url": "http://example.com",
  "review": "positive"
}

