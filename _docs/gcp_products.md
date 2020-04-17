---
layout: default
category: Platforms - GCP
title: Products
order: 1
permalink: /gcp_products.html
---

# Google Cloud Platform (GCP) Product Overview

## Before Getting Started...

This page covers some of the core services Google Cloud Platform offers.

Before getting started, [register for a GCP account](https://console.cloud.google.com/freetrial/signup/tos?pli=1). Each new user gets $300 in account credits to spend in the first 12 months.

## Common Products

### Virtual Machines

GCP offers virtual machines for general use; the most commonly used service is [Compute Engine](https://cloud.google.com/compute).

GCP also has specialized offerings for specific types of workloads. This includes services which have dedicated GPUs and can perform computationally intensive tasks. A complete look at the VM product suite is available [here](https://cloud.google.com/products/compute).

### Functions

[Cloud Functions](https://cloud.google.com/functions) are used to run custom code when asked, without having to provision, maintain, and pay for the infrastructure it runs on. It is a close analogue to AWS Lambda.

### Databases

GCP has many database solutions for different needs, and the most common ones are summarized below:

* [Cloud SQL](https://cloud.google.com/sql) is a fully managed relational database solution.
* [Firestore](https://cloud.google.com/firestore) is a NoSQL key-value database solution which was created with mobile, web, and IoT applications in mind.
* [Bigtable](https://cloud.google.com/bigtable) is a NoSQL key-value database solution which is ideal for large (petabyte-scale) datasets, data analytics, and machine learning.

For a full list of database offerings in GCP, see the [GCP Databases Overview page](https://cloud.google.com/products/databases).

### File Storage

#### Object Storage

[Cloud Storage](https://cloud.google.com/storage) is used to store, back up, and retrieve files, videos, logs, and anything else you can imagine. It is a close analogue to AWS Simple Storage Service (S3).

You can serve a static website via Cloud Storage in a similar fashion to S3; check out the [GCP static website tutorial](https://cloud.google.com/storage/docs/hosting-static-website) for more information.

#### Block Storage

Dedicated file storage for one or more virtual machines is provided through [Persistent disk](https://cloud.google.com/persistent-disk).

### Application Development

#### Mobile Apps

[Firebase](https://firebase.google.com/) is the seminal GCP service for creating, deploying, and managing mobile applications. It includes features like:

* Native integration with GCP services like Firestore, Cloud Functions, and Cloud Storage
* Testing for phsyical Android and iOS devices with Test Lab
* App crash reporting and analysis

#### Web Apps

[App Engine](https://cloud.google.com/appengine) is a fully-managed, highly scalable service which deploys an application you write (in any common language) to the web. 

App Engine is particularly useful for writing web applications and supporting an API-based backend for mobile applications. App Engine integrates tightly with Firebase for seamless developer and user experiences.

For more information about setting up App Engine, check out the [how-to guides in the App Engine documentation](https://cloud.google.com/appengine/docs/standard/python3).

### Security and Permissions

Read about GCP permissions management with IAM on [our documentation page]({% link _docs/gcp_secrets.md %}).

## Power User Products

GCP has a product suite for common needs for large businesses and enterprise shops. The most notable product families and their core services are listed here for reference.

### Machine Learning

GCP offers general-purpose and specialized services for machine learning applications.

* [AutoML](https://cloud.google.com/automl) is a general-purpose product for training and deploying machine learning models in GCP.
* [AI building blocks](https://cloud.google.com/products/ai/building-blocks) supports easy integration of machine learning into other GCP applications and projects.
* [Vision AI](https://cloud.google.com/vision) and [Video AI](https://cloud.google.com/video-intelligence) facilitate model training and deployment for computer vision problems.
* [Natural Language](https://cloud.google.com/natural-language) provides core capabilities of natural language processing in the cloud.
* [TTS](https://cloud.google.com/text-to-speech), [STT](https://cloud.google.com/speech-to-text), and [Dialogflow](https://cloud.google.com/dialogflow) enable human-computer interaction for a variety of platforms and applications.

View the full suite of machine learning offerings on the [GCP website](https://cloud.google.com/products/machine-learning).

### Data Analytics

For large-scale data analytics, GCP offers several key services which are purportedly more cost-effective than many competing options.

* [BigQuery](https://cloud.google.com/bigquery) enables low-latency, cost effective data analysis for large (petabyte-scale) datasets.
* [Dataflow](https://cloud.google.com/dataflow) provides batch processing functionality for dataset queries, while abstracting away server management from the scientists and engineers who are writing and running queries.
* [Dataproc](https://cloud.google.com/dataproc) creates open-source data analysis solutions, including native support of Apache Spark, Apache Hadoop, and other major platforms.
