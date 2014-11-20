---
layout: post
title:  "Exploring Kaggle Data with Google BigQuery"
date:   2014-11-05 23:50:00
categories: intro misc
---
One of the most important steps in any Kaggle competition is exploratory analysis. Before you start building models it's a good idea to spend some time getting to know the dataset you're working with. 

The problem is Kaggle datasets can be massive. If you, like me tend to do most of your work on a laptop, running aggregations on a 12GB file can be difficult and slow.

In this article I'll show you how Google BigQuery can be used as part of your Kaggle workflow to quickly analyze, sample, and run statistics on large datasets such as those encountered in Kaggle competitions. 

## Intro to Google Bigquery

[Google BigQuery](https://cloud.google.com/bigquery/) is a fast, cloud-based tool for quickly analyzing huge datasets using SQL-like queries. While it's not free to use, the cost is pretty reasonable and is a function of how much data you have stored, and how much you data you use in the queries you run. At this time, pricing is as follows:

- Storage: $0.02/month for each GB of data stored.
- Querying: $5 per Terabyte 

Example: The training dataset for the [Avazu](http://www.kaggle.com/c/avazu-ctr-prediction/data) contest, loaded into Bigquery, takes up 11.3 GB. This would cost you about $0.22 monthly to store in BigQuery. How much you're billed for queries depends on the size of the data queried. For instance if you run a query that requires 1 GB to execute, that will run you $0.005 (half a penny.) More pricing info can be found [here](https://cloud.google.com/bigquery/pricing).

Google provides several ways for uploading data and running queries. These include:

- Browser Tool: Great for getting started, and it has some handy features for saving queries, creating views, and query history.
- A simple command line tool ([bq](https://cloud.google.com/bigquery/bq-command-line-tool))
- A [RESTful API](https://cloud.google.com/bigquery/docs/reference/v2/), for when you want to integrate BigQuery into your application.

In this tutorial I'll cover just the first 2 (GUI and bq) methods.

## Step 1: Create a Google APIs account and activate services

It's pretty easy to get started. Just go to the [BigQuery](https://cloud.google.com/bigquery/) page on the Google Cloud Platform and create an account. Be sure to activate the BigQuery service. You may also want to activate the Google Compute Engine service if you plan to upload your data per the instructions in step 2 below.

Once everything's set up, you might want to try testing things out using the sample datasets provided. [This Quickstart Guide](https://cloud.google.com/bigquery/browser-tool-quickstart) is a great tutorial for 

## Step 2 (optional): Start up a Google Compute Engine Virtual Machine
This is optional, but it'll save you a lot of time when you start moving big datasets between Kaggle and BigQuery. By following the steps in this section, you'll be able to upload your data directly, rather than first downloading to your local hard drive and then uploading.

1. Fire up a Virtual Machine Instance. A g1-small instance type should be enough.
2. Open up a secure shell (SSH) window to your instance. Google provides a simple in-browser SSH client

## Step 3: Install the Google Cloud SDK

Install the Google Cloud SDK by following the steps [here](https://cloud.google.com/sdk/.) If you're planning to upload the files from your local computer, install the SDK locally. If you took the time to start up a Virtual Machine in Step 2, install the SDK on your VM using the same steps

## Step 4: Upload Data

If you *did not* complete step 2, go ahead and download the Avazu Training Dataset to your local computer, via your browser, from the [Competition Page](https://www.kaggle.com/c/avazu-ctr-prediction/data)

If you did take the time to set up a Google Compute Engine Virtual Machine, fetch the data using cURL on the VM. I've found the easiest way to do this using the "copy to cURL" feature available in Chrome Developer Tools and Firebug for Firefox. t for Firefox: 

- If you have Google Chrome installed, pull up Developer Tools, click the "Network" tab, and then click on the link to the datafile. More details [here](http://www.lornajane.net/posts/2013/chrome-feature-copy-as-curl)
- If you use Firefox, install the [Firebug plugin](http://getfirebug.com/). Info on copying as cURL in Firebug can be found [here](http://www.softwareishard.com/blog/planet-mozilla/firebug-tip-resend-http-request/)

Once you've "copied to cURL", paste the command into your VM shell and hit enter.

[insert screenshot here...]

Once you have the dataset, you're ready to upload to BigQuery!  

{% highlight bash %}
zcat avazu_train_rev2.gz | head -n 1 > schema.txt
gsutil cp avazu_train_rev2.gz gs://kaggle_contests/avazu_train_rev2.gz
bq load --skip_leading_rows=1 kaggle_avazu.train gs://kaggle_contests/avazu_train_rev2.gz "`cat schema.txt`"
{% endhighlight %}

## Step 5: Lets run some queries!

Walk thru a 2-3 queries.

## Other options

In future post I'll talk about ways to visually explore this dataset using Tableau.
