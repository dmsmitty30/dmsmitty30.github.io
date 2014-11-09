---
layout: post
title:  "Exploring Kaggle Data with Google BigQuery"
date:   2014-11-05 23:50:00
categories: intro misc
---
I recently started competing in Kaggle competitions, mostly as a way to build my own understanding of predictive analytics and machine learning. One of the most important steps in any Kaggle competition is exploratory analysis. Before you start building models it's a good idea to spend some time getting to know the dataset you're working with. 

Problem is Kaggle datasets can be massive. If you, like me tend to do most of your work on a laptop, running aggregations on a 12GB file can be difficult and slow.

## Create a Google APIs account and activate services

Talk about how to go about setting up an account. Be brief. link to Google APIs site.
Show an example "hello world" query using the Shakespear dataset.

## (optional) Start up a Google Compute Engine Virtual Machine
Spin up a Virtual Machine.
Install gsutil, bq, etc.
Explain why you would want to do this.

## Upload Data
Use the Avazu dataset. show how to create schema and upload using bq command line tool.


## Explore!

Walk thru a 2-3 queries.

## Other options

In future post I'll talk about ways to visually explore this dataset using Tableau.
