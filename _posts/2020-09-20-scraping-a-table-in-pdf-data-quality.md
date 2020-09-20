---
title: "Scraping a table in a PDF and then test the data quality in Python"
excerpt: "How to scrape a table within a PDF in Python, unit test the data for quality and then upload it to S3."
date: 2019-07-13
header:
  overlay_image: "https://cdn-images-1.medium.com/max/1600/1*i6Bix6qoKU0ZMdLDVfA3ww.jpeg"
  overlay_filter: 0.5 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Tim Mossholder**](https://unsplash.com)"
tags:
  - AWS
  - Data Engineering
  - Python
---

## Introduction

Suppose you need to ingest some data into your data warehouse and after further discussions with your stakeholders the source of this data is a PDF document. Fortunately, this is pretty easy to do using a Python package called tabula-py. In this article I'm going to walk you through how you can scrape a table embedded in a PDF file, unit test that data using Great Expectations and then if valid, save the file in S3 on AWS. You can find the full source code to this article on Github or a working example on Google Colab.

## 0) Required Dependencies

Before writing any code, let's install all required packages.

```python
pip install tabula-py
pip install great_expectations
pip install boto3
```

## 1) Required Imports

```python
from tabula import read_pdf
import great_expectations as ge
import boto3
from io import StringIO
```

## 2) Read the PDF files

Below I have two PDF files which include some data related to some SpaceX launches. One is a clean and the expected format and the other has intentional errors which we'll use when we unit test our date output.

```python
clean = "https://raw.githubusercontent.com/AshHimself/etl-pipeline-from-pdf/master/spacex_launch_data.pdf"
messy = "https://raw.githubusercontent.com/AshHimself/etl-pipeline-from-pdf/master/spacex_launch_mess_data.pdf"
source_pdf = read_pdf(clean, lattice=True, pages="all")
df = source_pdf[0]
```

## 3) Review Data

Let's take a look at our data to see how well tabula-py worked using only a single line of code!

```python
df.head()
```

![enter image description here](https://cdn-images-1.medium.com/max/1600/1*0GWeY1pJ2IRuxNIlB8hMiw.png)

Pretty impressive! The only noticeable issue is that those headings won't work very well in a database so let's clean them up a little.

```python
#Rename the headers
fields = {'Flight Numb': 'flight_number', 'Dr ate': 'date','Time (UTC)': 'time_utc','Booster Ver':'booster_version','iLoanunch Site':'launch_site'}
df = df.rename(columns=fields) #rename columns
df.columns = map(str.lower, df.columns) ##lower case is nicer
```

![enter image description here](https://cdn-images-1.medium.com/max/1600/1*nM7KVrV6IXs1d8pP6kU2ag.png)

So far we have scraped the table from the PDF, saved it within a Pandas data frame and if we wanted to, we could save the data frame to a CSV on S3. However, what if the format of this PDF changes, what if someone changes a field name, how can we guarantee the data will be saved in S3 reliably?

## 4) Assert data

To test our data we are going to use an amazing package called Great Expectations. I'm not even going to be touching the surface of what this package can do so I highly suggest checking it out!.

```python
df_ge = ge.dataset.PandasDataset(df) #Load pd dataframe into great_expectations (GE)

#Define a rule to define the order and fields that should exist
df_ge.expect_table_columns_to_match_ordered_list(['flight_number',
 'date',
 'time_utc',
 'booster_version',
 'launch_site',
 'payload',
 'customer',
 'outcome'])
#flight_number should ALWAYS be unique. We don't want duplicate data in our warehouse
df_ge.expect_column_values_to_be_unique('flight_number')

#flight_number should never be null
df_ge.expect_column_values_to_not_be_null('flight_number')

#flight_number should be an integer
df_ge.expect_column_values_to_be_of_type('flight_number', 'int64')

#Validate our data
validation_results = df_ge.validate()

#If our data is valid validation_results["success"] should return True
if(validation_results["success"] == True):
    print ('All assertions have passed! :)')
else:
    print ('Some assertions have failed! :(')
```

If we ran the code now we now, using our expected pdf, it should pass all assertions. However, take a look at the messy pdf. I have intentially added some duplicate flight_number rows and renamed the output column. If we ran it again (updating the PDF source from section 2) using the messy PDF the above code should return "Some assertions have failed :(". The output of validation_results is also fairly verbose but you can view the full output here.

```python
{"Expected Column Position": 7,"Expected": "outcome","Found": "mission outcome"}
```

Having some basic tests in your ETL pipeline like this can easily help in ensuring the quality of your data is consistant and reliable.

## 5) Saving our data in S3

Its beyond the scope of this article to teach you how to setup an S3 bucket with correct permissions, so i'm going to assume you know how to do that or know how to figure it out.

```python
AWSAccessKeyId='your_access_key_id'
AWSSecretKey='your_secret_key'
bucket = 'scrape-pdf-example' # your S3 bucket
csv_buffer = StringIO()
s3 = boto3.client('s3', aws_access_key_id=AWSAccessKeyId, aws_secret_access_key=AWSSecretKey)
df.to_csv(csv_buffer) #save our dataframe into memory
s3_resource = boto3.resource('s3')
s3_resource.Object(bucket, 'df.csv').put(Body=csv_buffer.getvalue()) #push CSV to S3
```

Code snippet from Stefan!
Done! you should now have a CSV file in your S3 bucket, which if you wanted could be ingested into other AWS service (Glue, Redshift etc).

![enter image description here](https://cdn-images-1.medium.com/max/1600/1*OfjaPa4XOv1DZ4QoEBS7Zg.png)

    Do you test the quality of your data in production?

Closing Comments
The above code could have been easily achieved using the AWS Service Textract service or other Python packages, but for me, Tabula worked great on simple and more complex PDF documents with multiple tables and more complex table structures.
Data Quality and testing data in general is critical and often neglected. Solutions like DBT and Matillion offer functionality to test/assert your data and make this process extremely easy to do. What tools do you use?
Saving the final output to S3 is obviously optional. I only chose to do this as a future article will be making use of the CSV file in S3 as a starting point.
