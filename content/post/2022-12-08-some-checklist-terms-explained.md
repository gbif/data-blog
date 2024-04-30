---
title: Checklist publishing on GBIF - some explanations on taxonID, scientificNameID, taxonConceptID, acceptedNameUsageID, nameAccordingTo
author: Marie Grosjean
date: '2022-12-08'
slug: some-checklist-terms-explained
categories:
  - GBIF
  - data publishing
tags:
  - GBIF
  - checklist
  - publishing
lastmod: '2022-12-08'
draft: no
keywords: ['checklist', 'identifiers', 'circumscriptions', 'misapplied names']
description: ''
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


When data publishers publish checklists, they will use a a Darwin Core Archive `Taxon Core`. And although the taxon core terms are already described [here](https://dwc.tdwg.org/terms/#taxon), what exactly to put in which field can sometimes be confusing. And there is a lot to read, like here https://github.com/tdwg/tnc/issues/1

Here I am sharing a summary of an email conversation we had with some data publishers on Helpdesk concerning some of the Taxon Core fields. I hope this can be useful for others.

## General considerations

The **dwc:taxonID** field contains the identifier used to establish relationships between entries *within* a checklist. The **dwc:taxonID** field can be understood as a “nameUsageID”, that is, an identifier for a name as used in a specific context, here: the context of the checklist (it isn't called like that for historical reasons but this would match the concept better).
This means the **dwc:acceptedNameUsageID** field must refer to identifiers used in the **dwc:taxonID** field. For example, to signify that *Caretta nasuta* is a synonym of *Caretta caretta* you could have a checklist like this:

| taxonID | scientificName | acceptedNameUsageID | taxonomicStatus |
|---|---|---|---|
| ID1 | Caretta caretta (Linnaeus, 1758) |  | ACCEPTED |
| ID2 | Caretta nasuta Rafinesque, 1814 | **ID1** | SYNONYM |

Other fields such as **[dwc:parentNameUsageID](https://dwc.tdwg.org/terms/#dwc:parentNameUsageID)** link to **dwc:taxonID**s.

However, this doesn’t have to be the case for the **scientificNameID** and **taxonConceptID** fields

The **dwc:scientificNameID** is meant to identify the name alone and ideally uses an identifier from a nomenclator such as IPNI or ZooBank, but can also be a name identifier specific for the dataset. Our system doesn’t really interpret this field. [**Edit 2023-11-09:** the scientificNameID is now intepreted in specific contexts, if you want to know more, please read: https://github.com/gbif/pipelines/issues/217]

The **dwc:taxonConceptID** on the other hand is meant to identify the taxon concept regardless of which name is being used. [Avibase](https://avibase.bsc-eoc.org/avibase.jsp)IDs (like `D3A260BCA65503C6` for https://avibase.ca/D3A260BC) are a perfect fit for example. Even though we do not make use of them much, they could be used to identify identical concepts. 

**NB:** The values in the **dwc:taxonID** field for the [GBIF Backbone taxonomy checklist](https://doi.org/10.15468/39omei) correspond to the GBIF taxon keys used in species pages (like `https://www.gbif.org/species/2433753`) and to filter for GBIF occurrences (like `https://www.gbif.org/occurrence/search?taxon_key=2433753`). For any other checklist, the **dwc:taxonID** are likely to be different and unrelated to GBIF taxon keys. You can use any identifier in your own checklist.

## Relationship between those fields in two specific cases: circumscriptions and misapplied names

### For capturing possible circumscriptions:

For those of us who need a little reminder:
> In [biological taxonomy](https://en.wikipedia.org/wiki/Taxonomy_(biology)), **circumscription** is the content of a [taxon](https://en.wikipedia.org/wiki/Taxon), that is, the delimitation of which subordinate taxa are parts of that taxon. If we determine that species X, Y, and Z belong in Genus A, and species T, U, V, and W belong in Genus B, those are our circumscriptions of those two genera. Another systematist might determine that T, U, V, W, X, Y, and Z all belong in genus A. Agreement on circumscriptions is not governed by the Codes of [Zoological](https://en.wikipedia.org/wiki/International_Code_of_Zoological_Nomenclature) or [Botanical](https://en.wikipedia.org/wiki/International_Code_of_Botanical_Nomenclature) Nomenclature, and must be reached by scientific consensus.

See: https://en.wikipedia.org/wiki/Circumscription_(taxonomy)

How this would be translated into a checklist:
1. each entry in the checklist dataset will have its own **dwc:taxonID**
2. multiple entries of the accepted name corresponding to the different circumscriptions would have the same **dwc:scientificNameID**
3. but each would have a distinct **dwc:taxonConceptID**
4. use **dwc:nameAccordingTo** to refer to the source of the circumscription.

In practice, how that this look like?

Let's try to apply this to the genus *Cortinarius* (Pers.) Gray. According to Liimatainen, K., Kim, J.T., Pokorny, L. *et al.* (https://doi.org/10.1007/s13225-022-00499-9), the family *Cortinariaceae*  should be split into ten genera. Following that assessment, the concept associated with the name *Cortinarius* became more narrow as many species have been moved to different genera like *Calonarius callochrous* (Pers.) Niskanen & Liimat., 2022.

![Fig. 3 From: Taming the beast: a revised classification of Cortinariaceae based on genomic data  |373x500](/post/2022-12-08-some-checklist-terms-explained/fungi-revised.jpg)

So if I want to model this idea in a checklist, I could do as following (here I am using the [identifier from Index Fungorum](http://www.indexfungorum.org/Names/NamesRecord.asp?RecordID=17391) as scientificNameID):

| taxonID | scientificName | dwc:nameAccordingTo  | scientificNameID  | dwc:taxonConceptID |
|---|---|---|---|--|
| ID1 | Cortinarius (Pers.) Gray |  | urn:lsid:indexfungorum.org:names:17391 | conceptID-1 |
| ID2 | Cortinarius (Pers.) Gray | Niskanen & Liimat., 2022 | urn:lsid:indexfungorum.org:names:17391 | conceptID-2 | 

Note that we could also include the sensu/sec information in the **dwc:scientificName**. So the scientific name for the ID2 entry could also be `Cortinarius (Pers.) Gray. sec. Liimatainen, K., Kim, J.T., Pokorny, L. et al.` or `Cortinarius (Pers.) Gray. sensu Liimatainen, K., Kim, J.T., Pokorny, L. et al.`.

We could do something similar for *Tyrannausorus rex* which had many different circumscriptions over the years as many specimens originally identified as separate species were later thought to be juvenile T. rex. For example, Holtz identified *Aublysodon mirandus* as a young T. rex in 2004.

| taxonID | scientificName | dwc:nameAccordingTo  | scientificNameID  | dwc:taxonConceptID |
|---|---|---|---|--|
| ID1 | *Tyranosaurus rex* Osborn, 1905 | Rauhut 2003 | `https://paleobiodb.org/classic/checkTaxonInfo?taxon_no=54833` | conceptID-16 |
| ID2 | *Tyranosaurus rex* Osborn, 1905 | Holtz 2004 | `https://paleobiodb.org/classic/checkTaxonInfo?taxon_no=54833` | conceptID-18 | 

### For misapplied names:

> A misapplied  name  is when  the  name  of  one  taxon  is  erroneously  applied  to  a  different  taxon.

(From: https://doi.org/10.7560/740440-015)

How this would be translated into a checklist:
1. the entry for the misapplied name would have a unique **dwc:taxonID**,
2. the **dwc:taxonomicStatus** would be `misapplied`,
3. the **dwc:nameAccordingTo** would be used to specify the source being the author of the misapplication of the name,
4. the field **dwc:acceptedNameUsageID** would contain the ID of the currently accepted name in the dataset for the actual taxonomic entity corresponding to the correct identification.

For example:
| taxonID | scientificName | dwc:nameAccordingTo  | dwc:taxonomicStatus  | dwc: acceptedNameUsageID |
|---|---|---|---|--|
| ID1 | *Ficus exasperata* Vahl | Vahl. (1805). In: Enum. Pl. 2: 197. | ACCEPTED |  |
| ID2 | *Ficus exasperata* | De Wildeman & Durand in Ann. Mus. Congo Belge, B, Bot., ser. 2, 1: 54. 1899 | MISAPPLIED | ID1 |


I hope this post is useful. If you would like us to cover more checklist cases and examples, please leave us a comment letting us know what you would like to see.
