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
