---
layout: post
title: Moving Data with ODI Studio
---

I've recently been working on a project shuffling data from an Oracle 12g database into a SQL Server database and I wanted to get some thoughts on the project down somewhere so I can refer back to them.  The client is heavily invested in a product called ODI Studio and while I've never used this particular product, I have migrated data from one database to another.

ODI Studio has quite a learning curve, but once you get proficient with it, you can create data moving scenarios fairly quickly using many of the built in modules.  You can even modify the built in modules and create fairly powerful data migration modules if you are willing to look through the beanscript documentation for ODI.

I plan on creating a series of these blog posts outlining some of the approaches I took and some gotcha's that I ran across.  The first post is going to be about the general approach.

## Overview
I always approach data projects in a similar way.  In this case the newer database was the Oracle database and was the source of truth.  There were several other applications that needed to stay on the existing SQL Server database so the task is to do a nightly push of data from the Oracle database to the SQL Server database using ODI.

The schemas between the databases are very different so after analyzing the data, I first create a set of temporary tables in the source database that mimic the destination tables that I will be inserting into and updating.  So for instance if I have a table in the destination called Foo, I would create a table in the source database called Temp_Foo.  Since I'm dealing with different RDBMS's that have different data types, I approximate and do the best I can with types.  String types are fairly straightforward varchar2's in Oracle map over to varchar's in SQL Server.  Integers are interesting because they are very different in the two systems.  Numeric types in Oracle are very different from integers in SQL server.  Oracle data types dictate that you specify the maximum number of digits in a number, so if the max number you need to store is 9999 your data type will be Number(4,0).  The first parameter specifies the length in digits and second specifies the precision for decimal numbers.  Most of the columns in my SQL Server database were integers or string so this worked out rather nicely.  Since integer fields in SQL server can hold a max of 2,147,483,647, the field in my temp table needed to be Number(10,0) to hold all of the digits.

Once you have the fields in the temp table specified, you can use ODI Studio to start mapping fields over.  I'll get to this in a separate post.  This post is just about the general approach.  Putting data into the temp tables is done using Integrations in ODI Studio.  You will have at least one integration for each table in the destination database that you want to populate and keep up to date.  This is honestly where I did most of the hard work.  The whole point of creating the temp tables that mimic the destination tables is to leverage the power of ODI studio for doing what is essentially a batch of merge statements on the destination database.

The built in modules do a fairly good job of these insert/update/delete statements which are the next steps.

##Summary
1.  Create temp tables in your source database, 1 per table in your destination that you want to migrate data to.
2.  Create an integration per temp table that will select data from the source database that mimics the destination data format and insert that into the temp tables.
3.  Create an integration per temp table that will do inserts and updates in the destination tables based on data in the source temp tables.
4.  Create an integration per temp table that will do deletes in the destination tables based on data in the source temp tables.
