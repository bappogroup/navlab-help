---
description: 'When your data is ready, you may want to upload it using the migration tool.'
---

# How to Upload Data Using the CSV Migration Tool

## Bappo CSV Migration Tool

The Bappo Platform allows you to upload data directly from CSV files into your Application Database. The following instructional video explains how to do so.

{% page-ref page="how-to-upload-data-using-the-csv-migration-tool.md" %}

{% hint style="info" %}
The data files provided for your download in the [What You Need to Collect](what-you-need-to-collect.md) section are already set up for data migration uploads. As long as you don't change the column headings, include all mandatory fields, and provide the data in the correct format, there should be minimal issues.
{% endhint %}

## Understanding Data Relationships

Your data will often contain relationships. 

* Rates are related to carriers. 
* Cities are related to Countries.
* Staff Members are related to Departments.

It is important to realise that the relationships between data also determine the order in which you upload data. 

* You need to import Departments before Staff.
* You need to import Countries before Cities.

{% hint style="danger" %}
If you upload data in the incorrect order, some record uploads will fail.
{% endhint %}

We recommend that you upload the data in the following order:

* [ ] * [ ] * [ ] * [ ] 
