---
title: GBIF and Apache-Spark on Microsoft Azure tutorial 
author: John Waller
date: '2021-04-22'
slug: microsoft-azure-and-gbif
categories:
  - GBIF
tags:
  - cloud computing
  - azure
lastmod: '2021-04-22T16:00:05+02:00'
draft: yes
keywords: []
description: ''
authors: ''
comment: no
toc: ''
autoCollapseToc: no
postMetaInFooter: no
hiddenFromHomePage: yes
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

**GBIF** now has a [snapshot](https://github.com/microsoft/AIforEarthDataSets/blob/main/data/gbif.md) of 1.2 billion occurrences<sub>✝</sub> records on [Microsoft Azure](https://www.microsoft.com/en-us/ai/ai-for-earth). 

It is hosted by the **Microsoft AI for Earth program**, which hosts geospatial datasets that are important to environmental sustainability and Earth science. Hosting is convenient because you could now use occurrences in combination with other environmental layers, and not need to upload any of it to the Azure. 

The main reason you would want to use cloud computing over your own computer is because you can run **big data queries** that are slow or impractical on a local machine. 

<!--more-->

In this tutorial, I will be running a simple query on 1.2 billion occurrences records. I will be using [apache-spark](https://spark.apache.org/). Spark has APIs in **R**, **scala**, and **python**.  

<sub>✝</sub>The snapshots include all **CC-BY licensed data** published through GBIF that have coordinates which passed automated quality checks.

The GBIF mediated occurrence data are stored in Parquet files in Azure Blob Storage in the West Europe Azure region. The periodic occurrence snapshots are stored in occurrence/YYYY-MM-DD, where YYYY-MM-DD corresponds to the date of the snapshot. 

## Setup for running a databricks spark notebook on Azure

> You will be able to run free queries for a time, but eventually you will have to pay for your compute time. 

**Create a Microsoft Azure account** [here](https://azure.microsoft.com/en-us/free/)

I find it easiest to use the [az command line interface](https://docs.microsoft.com/en-us/cli/azure/). You can also set up using the Azure web interface. This tutorial is based off of the Databricks [quickstart guide](https://docs.microsoft.com/en-us/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal?tabs=azure-portal).

**Download az** [here](https://docs.microsoft.com/en-us/cli/azure/).

**Log into your account** and **install databricks**.

```shell
az login
az extension add --name databricks
```
Create a **resource group** `databricks-quickstart`. The GBIF snapshot is located in `westeurope`. 

```shell
az group create --name databricks-quickstart --location westeurope
az account list-locations -o table
```

Create a **workspace**: 

```shell
az databricks workspace create 
    --resource-group databricks-quickstart \
    --name mydatabrickws  \
    --location westeurope  \
    --sku standard
```

From this page [Azure web portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceGroups), click on **databricks-quickstart**. 

![launch workspace](/post/2021-04-22-microsoft-azure-and-gbif_files/resource_groups_screenshot.png)

Click on **launch workspace**.

![launch workspace](/post/2021-04-22-microsoft-azure-and-gbif_files/launch_workspace.png)

Click on **new cluster**.

![screenshot](https://docs.microsoft.com/en-us/azure/databricks/scenarios/media/quickstart-create-databricks-workspace-portal/databricks-on-azure.png)

Create a **new cluster**.

![screenshot](https://docs.microsoft.com/en-us/azure/databricks/scenarios/media/quickstart-create-databricks-workspace-portal/create-databricks-spark-cluster.png)

Click **create blank notebook**. Select the **default language you want to use** (R, scala, python).

![launch workspace](/post/2021-04-22-microsoft-azure-and-gbif_files/blank_notebook.png)

You should now be able to run the code below in **your new notebook**. 

![launch workspace](/post/2021-04-22-microsoft-azure-and-gbif_files/scala_notebook.png)

In this code chunk, I will **count the number of species (specieskeys) in each country**.

```scala
import org.apache.spark.sql.functions._

val gbif_snapshot_path = "wasbs://gbif@ai4edataeuwest.blob.core.windows.net/occurrence/2021-04-13/occurrence.parquet/*"

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

The resulting `export_df` should look like this. 

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

Here is how you would count species by country using **SparkR**. 

```r 
library(magrittr) # for %>% 
install.packages("/databricks/spark/R/pkg", repos = NULL)
library(SparkR)

sc = sparkR.init()
sqlContext = sparkRSQL.init(sc)

gbif_snapshot_path = "wasbs://gbif@ai4edataeuwest.blob.core.windows.net/occurrence/2021-04-13/occurrence.parquet/*"

df = read.parquet(sqlContext,path=gbif_snapshot_path)
printSchema(df)

export_df = df %>% 
select("countrycode","specieskey") %>% 
distinct() %>%
groupBy("countrycode") %>% 
count() %>%
arrange("count",decreasing = TRUE) 

export_df %>% showDF()
```

How you would count species by country in **pyspark**.

```python
from pyspark.sql import SQLContext
from pyspark.sql.functions import *
sqlContext = SQLContext(sc)

gbif_snapshot_path = "wasbs://gbif@ai4edataeuwest.blob.core.windows.net/occurrence/2021-04-13/occurrence.parquet/*"

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

## Exporting a csv table

We would now like to export `export_df` from the previous section into a csv file, so we can analyze in on our own computer. **There is also a bit of setup involved here**. 

It is easiest to use the command line tool `az` ( [download here](https://docs.microsoft.com/en-us/cli/azure/) ) to set up **storage accounts** and **containers** for storing your exported csv. 

Replace `jwaller@gbif.org` with your Microsoft-Azure account name. 

```shell
az login
az storage account create -n gbifblogstorage -g databrick-quickstart -l westeurope --sku Standard_LRS

az role assignment create --role "Owner" --assignee "jwaller@gbif.org"
az role assignment create --role "Storage Blob Data Contributor" --assignee "jwaller@gbif.org"
az role assignment create --role "Storage Queue Data Contributor" --assignee "jwaller@gbif.org"

az storage container create -n container1 --account-name gbifblobstorage --auth-mode login
```

Run this command to get the **secret key** (`sas_key`) you will need in the next section. 

```shell
az storage account keys list -g databrick-quickstart -n gbifblobstorage
```

The **secret key** (`sas_key` in next section) you want is under `value`. 

```
{
"keyName": "key1",
"permissions": "FULL",
"value": "copy_me_big_long_secret_key_kfaldkfalskdfj203932049230492f_fakekey_j030303fjdasfndsafldkj=="
}
```

Now that the set up is over, you can **export a dataframe** as a csv file. The **save path** has the following form: 

```
wasbs://container_name@storage_name.blob.core.windows.net/file_name.csv
```

We will use **container1** and **gbifblobstorage** created earlier. Copy your `sas_key` to replace my fake one. Run this in one of the notebooks you set up earlier. 

```scala
val sas_key = "copy_me_big_long_secret_key_kfaldkfalskdfj203932049230492f_fakekey_j030303fjdasfndsafldkj==" // fill the secret key from the previous section 
spark.conf.set("fs.azure.account.key.gbifblobstorage.blob.core.windows.net",sas_key)

// or you could export a parquet file
// export_df.write.parquet("wasbs://container1@gbifblobstorage.blob.core.windows.net/export_df.parquet")

export_df.
coalesce(1).
write.
mode("overwrite").
option("header", "true").
option("sep", "\t").
format("csv").
save("wasbs://container1@gbifblobstorage.blob.core.windows.net/export_df.csv")

```

```r 
library(SparkR)
sparkR.session()

sas_key = "copy_me_big_long_secret_key_kfaldkfalskdfj203932049230492f_fakekey_j030303fjdasfndsafldkj=="
sparkR.session(sparkConfig = list(fs.azure.account.key.gbifblobstorage.blob.core.windows.net = sas_key))

export_df %>%
repartition(1) %>%
write.df("wasbs://container1@gbifblobstorage.blob.core.windows.net/export_df.csv", "csv", "overwrite")
```

```python
val sas_key = "copy_me_big_long_secret_key_kfaldkfalskdfj203932049230492f_fakekey_j030303fjdasfndsafldkj==" // fill the secret key from the previous section 
spark.conf.set("fs.azure.account.key.gbifblobstorage.blob.core.windows.net",sas_key)

export_df\
.coalesce(1)\
.write\
.mode("overwrite")\
.option("header", "true")\
.option("sep", "\t")\
.format("csv")\
.save("wasbs://container1@gbifblobstorage.blob.core.windows.net/export_df.csv")
```

You can now download the `export_df.csv` from the azure **web interface**. The file will not be a single csv file but a directory named **export_df**. Within this directory you will find several files. The one you are interested in will be something looking like this: 

```
export_df.csv/part-00000-tid-54059299456474431-7c74a0a3-2332-423c-89ed-2d1a98427a01-449-1-c000.csv
```

## Downloading the csv

Go to your [storage account](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts). 

Click on **gbifblobstorage**.

![gbifblobstorage](/post/2021-04-22-microsoft-azure-and-gbif_files/gbifblobstorage.png)

Click on **containters**.

![gbifblobstorage](/post/2021-04-22-microsoft-azure-and-gbif_files/containers.png)

Download the **csv**.

`export_df.csv/part-00000-tid-54059299456474431-7c74a0a3-2332-423c-89ed-2d1a98427a01-449-1-c000.csv`.

![gbifblobstorage](/post/2021-04-22-microsoft-azure-and-gbif_files/part.png)

## Citing custom filtered/processed data

If you end up using your **exported csv** file in a research paper, you will want a **doi**. GBIF now has a [service](https://www.gbif.org/citation-guidelines) for generating a **citable doi** from **a list of involved datasetkeys with occurrences counts**. See the [GBIF citation guidelines](https://www.gbif.org/citation-guidelines). 

You can generate a **citation file** for our request above using the following code chunk. Since our `export_df.csv` used all of the occurrences, we can simply group by datasetkey and count all of the occurrences to generate our citation.csv file. 

```scala
val sas_key = "copy_me_big_long_secret_key_kfaldkfalskdfj203932049230492f_fakekey_j030303fjdasfndsafldkj==" // fill the secret key from the previous section 
spark.conf.set("fs.azure.account.key.gbifblobstorage.blob.core.windows.net",sas_key)

import org.apache.spark.sql.functions._

val gbif_snapshot_path = "wasbs://gbif@ai4edataeuwest.blob.core.windows.net/occurrence/2021-04-13/occurrence.parquet/*"

val citation_df = spark.read.parquet(wasbs_path).
groupBy("datasetkey").
count()

citation_df.
coalesce(1).
write.
mode("overwrite").
option("header", "true").
option("sep", "\t").
format("csv").
save("wasbs://container1@gbifblobstorage.blob.core.windows.net/citation_df.csv")
```

Hopefully you have everything that you need to start using GBIF mediated occurrence data on Microsoft Azure. Please leave a comment if something does not work for you. 


