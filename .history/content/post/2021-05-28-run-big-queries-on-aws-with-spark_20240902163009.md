---
title: GBIF and Apache-Spark on AWS tutorial
author: John Waller
date: '2024-09-02'
slug: aws-and-gbif
categories:
  - GBIF
tags:
  - cloud computing
  - aws
lastmod: '2021-04-22T16:00:05+02:00'
draft: no
keywords: []
description: ''
authors: ''
comment: no
toc: ''
autoCollapseToc: no
postMetaInFooter: no
hiddenFromHomePage: no
contentCopyright: no
reward: no
mathjax: no
mathjaxEnableSingleDollar: no
mathjaxEnableAutoNumber: no
hideHeaderAndFooter: no
flowchartDiagrams:
  enable: no
  options: ''
sequenceDiagrams:
  enable: no
  options: ''
---

> Some queries in the blog post can be done for free using a new feature called [GBIF SQL downloads](https://data-blog.gbif.org/post/2024-06-24-gbif-sql-downloads/). 

**GBIF** now has a [snapshots](https://registry.opendata.aws/gbif/) of billions occurrence<sub>✝</sub> records on **Amazon Web Services** (AWS). This guide will take you through running **Spark notebooks** on AWS. The GBIF snapshot is documented : [here](https://github.com/gbif/occurrence/blob/master/aws-public-data.md).

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">June snapshot of <a href="https://t.co/CJaPsifdp0">https://t.co/CJaPsifdp0</a> occurrence data now available on the Amazon and Microsoft clouds, based on <a href="https://t.co/aGbvTisapJ">https://t.co/aGbvTisapJ</a>. See <a href="https://t.co/lRXM2uqFh0">https://t.co/lRXM2uqFh0</a> for more details.</p>&mdash; GBIF (@GBIF) <a href="https://twitter.com/GBIF/status/1400009135362646021?ref_src=twsrc%5Etfw">June 2, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

You can **read previous discussions about GBIF and cloud computing** [here](https://discourse.gbif.org/t/gbif-exports-as-public-datasets-in-cloud-environments/1835). The main reason you would want to use cloud computing is to run **big data queries** that are slow or impractical on a local machine. 

<!--more-->

In this tutorial, I will be running a simple query on billions of occurrences records. I will be using [apache-spark](https://spark.apache.org/) with the **Scala** and **Python** APIs. This guide is similar to the one written previously about [Microsoft Azure](https://data-blog.gbif.org/post/microsoft-azure-and-gbif/). You can also work with the snapshots using **SQL** [example here](https://github.com/gbif/occurrence/blob/master/aws-public-data.md#getting-started-with-athena).  

<sub>✝</sub>The snapshot includes all records shared under CC0 and CC BY designations published through GBIF that have coordinates which have passed automated quality checks. The GBIF mediated occurrence data are stored in Parquet files in AWS s3 storage in [several regions](https://github.com/gbif/occurrence/blob/master/aws-public-data.md).

## Running Apache-Spark on AWS

**Create an AWS account** : [here](https://aws.amazon.com/)

> You will be able to run free queries for a time, but eventually you will have to pay for your compute time. 

**Sign into the console account**. I logged in as the root user.

![root user](/post/2021-05-31-aws-and-gbif_files/root_user.png)

In the services drop down (there are a lot of services), find and **click on EMR**. 

![find emr](/post/2021-05-31-aws-and-gbif_files/find-emr.png)

Next click **create a cluster**.

![create cluster](/post/2021-05-31-aws-and-gbif_files/create-cluster.png)

**Click on advanced options** and **configure your cluster**.

![advanced options](/post/2021-05-31-aws-and-gbif_files/cluster-settings.png)

Make sure to select:

* **Livy**
* **Spark**
* **JupyterHub**

You can keep also these selected: Hadoop, Hive, Pig, Hue.

I used **emr-6.1.0**. I found **some other emr versions didn't work**. You might want to use the latest emr version. Give your cluster a name. I called mine `gbif_cluster`. **I kept all of the other default settings**. The cluster will take a few minutes to start up. Make sure to terminate your cluster when you are finished with this tutorial because Amazon will charge you even if your cluster is in the "Waiting" state. 

<!-- ## Export a csv table -->

<!-- If you would like to export `export_df` from the previous section into a csv file for download, you need to set up a **s3 bucket**.  -->

AWS now requires you to set up an **s3 bucket** to store your data before creating a workspace.

**Go to s3**. In the services drop down (there are a lot of services), find and **click on S3**.

![find s3](/post/2021-05-31-aws-and-gbif_files/find-s3-bucket.png)

**Create a s3 bucket**.  

![gbif bucket](/post/2021-05-31-aws-and-gbif_files/gbif-bucket.png)

**Give your s3 bucket a globally unique name**. I have used "**gbif_bucket**", so you will have to pick another one. Keep all of the default settings. Now you can **run the following code in one of your notebooks** to export a csv file to your s3 bucket. 


Next **Create a workspace (notebook)**. 

![create notebook](/post/2021-05-31-aws-and-gbif_files/create_notebook.png)

**Choose a cluster** for your notebook. Choose the `gbif_cluster` from before. 

![choose cluster](/post/2021-05-31-aws-and-gbif_files/choose_a_cluster.png)

**Name your notebook.** I called mine `gbif_notebook`. 

![name notebook](/post/2021-05-31-aws-and-gbif_files/name_notebook.png)

**Open your notebook**. "Open in JupyterLab".

![open jupyter](/post/2021-05-31-aws-and-gbif_files/open_jupyter.png)

You are now ready to run the examples below in your notebook. I will be running examples using the **Python** and **Scala** APIs. 

**Choose notebook kernel** (Spark or PySpark).

![choose notebook api](/post/2021-05-31-aws-and-gbif_files/choose_notebook_language.png)

**Paste in one of the following code chunks below**. They will both produce the same output. The code chunks will **count the number of species (specieskeys) with occurrences in each country**. Press shift-enter to run it. 

![code chunk](/post/2021-05-31-aws-and-gbif_files/code_chunk.png)

Use the **Spark** kernel for this code chunk.

```scala
import org.apache.spark.sql.functions._
val gbif_snapshot_path = "s3://gbif-open-data-eu-central-1/occurrence/2021-06-01/occurrence.parquet/*"
val df = spark.read.parquet(gbif_snapshot_path)

 
val df = spark.read.parquet(gbif_snapshot_path)
df.printSchema // show columns
df.count() // count the number of occurrences. 1.2B

// find the country with the most species
val export_df = df.
select("countrycode","specieskey").
distinct().
groupBy("countrycode").
count().
orderBy(desc("count"))

export_df.show()
```

Use the **Pyspark** kernel for this code chunk. 

```python
from pyspark.sql import SQLContext
from pyspark.sql.functions import *
sqlContext = SQLContext(sc)

gbif_snapshot_path = "s3://gbif-open-data-eu-central-1/occurrence/2021-06-01/occurrence.parquet/*"

# to read parquet file
df = sqlContext.read.parquet(gbif_snapshot_path)
df.printSchema # show columns

# find the country with the most species
export_df = df\
.select("countrycode","specieskey")\
.distinct()\
.groupBy("countrycode")\
.count()\
.orderBy(col("count").desc())

export_df.show()
```
The result should be a table like this: 

```
+-----------+------+
|countrycode| count|
+-----------+------+
|         US|187515|
|         AU|122746|
|         MX| 86399|
|         BR| 69167|
|         CA| 64422|
|         ZA| 60682|
```

## Export a csv table

If you would like to export `export_df` from the previous section into a csv file for download, you need to set up a **s3 bucket**. 

**Go to s3**. In the services drop down (there are a lot of services), find and **click on S3**.

![find s3](/post/2021-05-31-aws-and-gbif_files/find_s3.png)

**Create a s3 bucket**. You will  put your csv table here. 

![gbif bucket](/post/2021-05-31-aws-and-gbif_files/gbif_bucket.png)

**Give your s3 bucket a globally unique name**. I have used "**gbifbucket**", so you will have to pick another one. Keep all of the default settings. Now you can **run the following code in one of your notebooks** to export a csv file to your s3 bucket. 

Change `gbifbucket` to your bucket name. 

```scala
import spark.implicits._

export_df.
coalesce(1).
write.
mode("overwrite").
option("header", "true").
option("sep", "\t").
format("csv").
save("s3a://gbifbucket/df_export.csv")

```

```python
export_df\
.coalesce(1)\
.write\
.mode("overwrite")\
.option("header", "true")\
.option("sep", "\t")\
.format("csv")\
.save("s3a://gbifbucket/df_export.csv")
```

To download. Go to your [S3 bucket](https://s3.console.aws.amazon.com/s3/home?). Your file will be a directory named "df_export". The file you want will look something like this: 

`part-00000-4c2e7420-b122-404b-b8c6-62adb07173e0-c000.csv`

## Citing custom filtered/processed data

If you end up using your **exported csv** file in a research paper, you will want a **DOI**. GBIF now has a [service](https://www.gbif.org/derived-dataset/register) for generating a **citable doi** from **a list of involved datasetkeys with occurrences counts**. See the [GBIF citation guidelines](https://www.gbif.org/citation-guidelines) and [previous blog post](https://data-blog.gbif.org/post/derived-datasets/). 

You can generate a **citation file** for your custom dataset above using the following code chunk. Since our `export_df.csv` used all of the occurrences, we can simply group by datasetkey and count all of the occurrences to generate our `citation.csv` file. 

```scala
import org.apache.spark.sql.functions._
val gbif_snapshot_path = "s3://gbif-open-data-eu-central-1/occurrence/2021-06-01/occurrence.parquet/*"

val citation_df = spark.read.parquet(gbif_snapshot_path).
groupBy("datasetkey").
count()

citation_df.
coalesce(1).
write.
mode("overwrite").
option("header", "true").
option("sep", "\t").
format("csv").
save("s3a://gbifbucket/citation.csv")
```

You can also now use your **citation file** with the **development version** of `rgbif` to create a derived dataset and a citable DOI, **although you would need to upload your exported dataset to Zenodo (or something similar) first**. 


```r
# pak::pkg_install("ropensci/rgbif") # requires development version of rgbif

library(rgbif)

citation_data = readr::read_tsv("citation.csv")

# use derived_dataset_prep() if you just want to test
derived_dataset(
citation_data = citation_data,
title = "Research dataset derived from GBIF snapshot on AWS",
description = "I used AWS and apache-spark to filter GBIF snapshot 2021-06-01.",
source_url = "https://zenodo.org/fake_upload",
user="your_gbif_user_name",
pwd="your_gbif_password"
)
```

The derived dataset with a **citable DOI** will appear on your [gbif user page](https://www.gbif.org/derived-dataset). 

Hopefully you have everything that you need to start using GBIF mediated occurrence data on AWS. Please leave a comment if something does not work for you. 

