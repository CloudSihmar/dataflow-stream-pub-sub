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

