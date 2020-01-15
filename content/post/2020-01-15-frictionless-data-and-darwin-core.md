---
title: Frictionless Data and Darwin Core
author: André Heughebaert
date: '2020-01-15'
slug: frictionless-data-and-darwin-core
categories:
  - GBIF
tags:
  - Frictionless Data
lastmod: '2020-01-15T09:37:51+01:00'
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

**Frictionless data** is about removing the friction in working with data through the creation of tools, standards, and best practices for publishing data using the Data Package standard, a containerization format for any kind of data. It offers specifications and software around data publication, transport and consumption. Similarly to Darwin Core, data resources are presented in CSV files while the data model is described in a JSON structure.

<!--more-->

> [Frictionless Data](https://frictionlessdata.io/) is an [Open Knowledge Foundation](https://okfn.org/) project which started over 10 years ago as a community-driven effort.

# How does Frictionless data differ from Darwin Core?

Being **domain agnostic**, Frictionless Data is open to all sorts of data, not just biodiversity related fields.
It allows **truly relational** model as you can express primary and foreign keys in each of your data resources -- bye-bye star schema!

The [specifications](https://frictionlessdata.io/specs/) are well documented, simple still flexible and less prone to misinterpretation.
A very nice feature is that **field constraints** can be added to the data model.
The <br>[software library](https://frictionlessdata.io/software/) offers a bunch of tools such as a Data reader/writer, Data validator and a Data pipelines framework. Some are provided as libraries for various languages, other are online tools.

# What is FrictionlessDarwinCore?

Thanks to [Frictionless Data Tool Fund](https://toolfund.frictionlessdata.io/), I developed a **Python** tool that automatically converts **Darwin Core Archive** into a **Frictionless Data Packages**.

The following use case will put the tool in context:
Let's assume an archaeologist wants to work on GBIF mediated data. She starts by defining a query on GBIF.org such as **give me all occurrences of Miturgidae of Belgium**. The query return 1.635 occurrence records involving 4 datasets and she download the results as a DarwinCore Archive, [DOI=10.15468/dl.athbcd](https://doi.org/10.15468/dl.athbcd).
Then she converts it with FrictionlessDarwinCore into a Data Package

```script
fdwca 0039602-191105090559680.zip MiturgidaeBE_DP.zip
```
From now on, she can use any Frictionless software to access the data.
[Goodtables](https://goodtables.readthedocs.io/en/latest/) is a validation tool that check compliance with Data package specifications (both schema and data).

```script
goodtables MiturgidaeBE_DP.zip
```
In an ideal world, it should succeed. But very often, it gives a bunch of useful errors that informs you some data constraints are in violation. All reported errors start with: [row,column] [error]
```script
DATASET
=======
{'error-count': 4047,
 'preset': 'nested',
 'table-count': 3,
 'time': 7.171,
 'valid': False}

TABLE [1]
=========
{'datapackage': 'MiturgidaeBE_DP.zip',
 'error-count': 3265,
 'headers': ['gbifID',
              'abstract',
...
              'repatriated'],
  'row-count': 1636,
  'schema': 'table-schema',
  'source': '/var/folders/19/mrkkz1tx34l98n1rs3qr3xr00000gn/T/tmp111dpxv7-datapackage/occurrence.txt',
  'time': 6.939,
 'valid': False}
...
[9,64] [enumerable-constraint] The value "PRESERVED_SPECIMEN" in row 9 and column 64 does not conform to the given enumeration: "['PreservedSpecimen', 'FossilSpecimen', 'LivingSpecimen', 'MaterialSample', 'Event', 'HumanObservation', 'MachineObservation', 'Taxon', 'Occurrence']"
[6,135] [type-or-format-error] The value "3535.0" in row 6 and column 135 is not type "integer" and format "default"
...
```

Let's see some of them:

* **enumerable-constraint**: This field value should be equal to one of the values in the enumeration constraint.
* **type-or-format-error**: The value can’t be cast based on the schema type and format for this field.
* **required-constraint**: This field is a required field, but it contains no value.
* **unique-constraint**: This field is a unique field but it contains a value that has been used in another row.
See Goodtables GitHub repo - [Data Quality Errors](https://github.com/frictionlessdata/goodtables-py#data-quality-errors) for a complete list of errors.

Fortunately, our researcher can fine tune these quality checks by modifying the constraints SHE WANTS in datapackage.json (unzip the data package first).

Here she wants to validate that year, month and day fields are present and have a reasonable value. Here is how it looks like in the datapackage.json.
```JSON
{
  "name": "year",
  "type": "integer",
  "rdfType": "http://rs.tdwg.org/dwc/terms/year",
  "constraints": {
    "required": true,
    "minimum": 1000,
    "maximum": 2050
  }
},
{
  "name": "month",
  "type": "integer",
  "rdfType": "http://rs.tdwg.org/dwc/terms/month",
  "constraints": {
    "required": true,
    "minimum": 1,
    "maximum": 12
  }
},
{
  "name": "day",
  "type": "integer",
  "rdfType": "http://rs.tdwg.org/dwc/terms/day",
  "constraints": {
    "required": true,
    "minimum": 1,
    "maximum": 31
  }
},
```
Running goodtables again will give this:
```script
goodtables MiturgidaeBE_DP/data_package.json
...
[2,103] [required-constraint] Column 103 is a required field, but row 2 has no value
[2,104] [required-constraint] Column 104 is a required field, but row 2 has no value
[2,105] [required-constraint] Column 105 is a required field, but row 2 has no value
[3,103] [required-constraint] Column 103 is a required field, but row 3 has no value
[3,104] [required-constraint] Column 104 is a required field, but row 3 has no value
...
[3,105] [type-or-format-error] The value "avril" in row 3 and column 105 is not type "integer" and format "default"
[4,105] [type-or-format-error] The value "avril" in row 4 and column 105 is not type "integer" and format "default"
[5,105] [type-or-format-error] The value "Avril" in row 5 and column 105 is not type "integer" and format "default"
```

Now, our researcher can access the occurrences(and verbatim) records through one of the **Data Package reader** libraries (in Java, Julia, Mathlab, Go, Panda PHP, Python, R, Ruby) or even directly import them into OpenRefine or SQL.

# Status of the conversion tool

FrictionlessDarwinCore v1.0 main features are:

* Support all standards DarwinCore terms
* Support default values in DarwinCore schema
* Enable Fields constraints: for further data validation, with goodtables
* Accept DarwinCore Archive from local path or URL
* Command line interface, no Python programming skill needed

Does it converts all DwCA? Honestly no, but it converts all GBIF.org downloads, all IPT produced archives and most of the others, such as those from PLAZI (27.000+) or Pangaea (7.000+).

# Future directions

The conversion tool is hopefully just a beginning and I hope it may help the TDWG/GBIF community reflection on Data exchange standards.
I'll be happy to discuss it on GBIF community forum or elsewhere.

For users, I will be very happy to integrate my Frictionless tool into GBIF.org with the possibility for users to directly download Data Packages.

For publishers, on the other side of the data pipeline, I would also encourage to use Frictionless Data because it breaks the DwC star-schema limitation.

Cross disciplinary interoperability will be achieved when all discipline silos will adopt a common data exchange system. Domain experts should continue to adopt, and maintain, domain specific vocabularies and ontologies. At the same time, data should be exposed to a wider audience (humans and machines) by using the most widely accepted standard.

# How to contribute

You are encouraged to contribute by identifying/reporting issues or incompatibilities and helping to solve them via the [tool github repository](https://github.com/frictionlessdata/FrictionlessDarwinCore/releases).



