# Code_Snippets
Collection of code snippets, development hacks,useful tips and tricks


## Contents

- [Python](#python)
- [AWS S3](#aws-s3)
  - [S3 Client](#s3-client)
  - [Get parquet file](#get-parquet-file)
  - [Send file to S3 ](#send-file-to-s3 )


# Python

### AWS S3 


#### S3 Client 
```python 
import boto3

s3 = boto3.client('s3', aws_access_key_id="AWS_ACCESS_KEY",
				  aws_secret_access_key="AWS_SECRET_KEY",
				  region_name="AWS_REGION")

```


#### Get parquet file 
```python
import pandas as pd

# get parquet from S3
parquet_object = s3.get_object(Bucket="AWS_BUCKET_NAME", Key=key_file)['Body'].read()
parquet_buffer = io.BytesIO(parquet_object)

# read parquet buffer
df = pd.read_parquet(parquet_buffer)
```


#### Send file to S3 
```python
# upload file to S3
s3.upload_file(file_path, "AWS_BUCKET_NAME", file_name)
```


#### Send Partitionned Parquet file to S3 
```python
# pip install awswrangler
import awswrangler as wr

bucket = "AWS_BUCKET_NAME"
path_file = f"s3://{bucket}/xxxx/parquet_file.parquet"

wr.s3.to_parquet(df,
				 path_file,
				 partition_cols=['column_name'], 
				 dataset=True,
				 mode="append", 
				 compression='snappy')
```
