---
title: "Definition of Done for a Data Pipeline, "
excerpt: "When is it Done! Done!"
date: 2019-07-13
header:
  overlay_image: "https://cdn-images-1.medium.com/max/1600/1*i6Bix6qoKU0ZMdLDVfA3ww.jpeg"
  overlay_filter: 0.5 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Tim Mossholder**](https://unsplash.com)"
tags:
  - Data Engineering
---

## Introduction

Suppose you need to ingest some data into your data warehouse and after further discussions with your stakeholders the source of this data is a PDF document. Fortunately, this is pretty easy to do using a Python package called tabula-py. In this article I'm going to walk you through how you can scrape a table embedded in a PDF file, unit test that data using Great Expectations and then if valid, save the file in S3 on AWS. You can find the full source code to this article on Github or a working example on Google Colab.

## 0) Required Dependencies

