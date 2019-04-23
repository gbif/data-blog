---
title: Molecular-based data on GBIF - Sharing your data
authors: Marie Grosjean
date: '2000-11-15'
slug: gbif-molecular-data
categories:
  - GBIF
tags:
  - molecular
  - DNA
  - eDNA
  - GBIF
  - MixS
  - GGBN
  - EBI
  - sequence
  - OTU
  - Darwin Core
  - howto
  - publish
lastmod: '2019-04-23'
keywords: ['molecular', 'metagenomics', 'eDNA', 'sequence']
description: ''
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

GBIF is trying to make it easier to share molecular-based data. In fact, this past year alone, we worked with [UNITE](https://unite.ut.ee) to integrate species hypothesis for fungi and with EMBL-[EBI](https://www.ebi.ac.uk) to publish 295 metagenomics datasets.

Unfortunately, the documentation is not as easy to follow. Although we have now an [FAQ](https://www.gbif.org/faq?question=how-can-i-publish-molecular-data-to-gbif) on the topic, I thought that anyone could use a blog post with some advice and examples.

Note that this blog post is not intended to be documentation. The information written is subject to change, feel free to leave comments if you have any question or suggestion.

<img src="https://www.publicdomainpictures.net/pictures/40000/velka/--1359709322C31.jpg#.XLcmtPqn2pQ.link" alt="Structure Of DNA - Public Domain" width="600"/>

# Type of molecular-based data on GBIF


The term `molecular-based data` refers to any type of data associated to some genetic material (DNA or RNA sequence, genotype, etc.).
These data can  be sorted in one of the two following categories:

* the genetic material comes from an **observable specimen** (whether it is macro or microscopic).
* the genetic material is the **only evidence of a given organism or community**, see for example metagenomics samples.

These two categories have different implications in terms of quality control but this will be the topic of an other post.

# Examples

The examples I will use in this post are the following:

* Frøslev T, Ejrnæs R (2018). BIOWIDE eDNA Fungi dataset. Danish Biodiversity Information Facility. Occurrence dataset https://doi.org/10.15468/nesbvx accessed via GBIF.org on 2019-04-17.
* PlutoF (2019). UNITE - Unified system for the DNA based fungal species linked to the classification. Version 1.2. Checklist dataset https://doi.org/10.15156/bio/587474 accessed via GBIF.org on 2019-04-17.
* Cox F, Newsham K, Robinson C, Sweetlove M (2019). Microbial Fungi in soils on different Sub-Antarctic islands. SCAR - Microbial Antarctic Resource System. Metadata dataset https://doi.org/10.15468/jekfdj accessed via GBIF.org on 2019-04-17. + [all the SCAR - Microbial Antarctic Resource System metadata-only datasets](https://www.gbif.org/dataset/search?type=METADATA&publishing_org=af290483-8639-4b58-87fb-a4824c65e577)
* MGnify (2017). EMOSE (2017) Inter-Comparison of Marine Plankton Metagenome Analysis Methods. Sampling event dataset https://doi.org/10.15468/re7eoi accessed via GBIF.org on 2019-04-17. + [MGnify datasets](https://www.gbif.org/dataset/search?publishing_org=ab733144-7043-4e88-bd4f-fca7bf858880)
* Telfer A (2018). Centre for Biodiversity Genomics - Canadian Specimens. University of Guelph. Occurrence dataset https://doi.org/10.15468/mbwnw9 accessed via GBIF.org on 2019-04-17.


# Choosing a dataset class

Data providers must organize their data into datasets in order to share them on GBIF. Four different dataset classes are supported: resources metadata, checklists, occurrence datasets and sampling-event datasets (see [this page](https://www.gbif.org/dataset-classes) for more details). For the rest of this post, I assume that you are familiar with the structures of Darwin Core Archives, please consult [this page](https://github.com/gbif/ipt/wiki/DwCAHowToGuide) for more information).

When choosing which dataset class would suit your data best, keep in mind that extensions can only describe the core file. For example:

* If you would like to share sequences for each occurrence, consider organizing your data as an occurrence dataset: the extension containing the sequences will attached to occurrences (see [BIOWIDE eDNA Fungi dataset](https://doi.org/10.15468/nesbvx).
* Sampling-event datasets allow to put a greater emphasis on sampling protocol. This is the class adopted by [Mgnify](https://www.gbif.org/publisher/ab733144-7043-4e88-bd4f-fca7bf858880) to map its metagenomics data.

Hopefully, the rest of this blog post might give you a better idea of what is possible.


# Linnaean classification and non-Linnaean classification

<img align="right" src="https://upload.wikimedia.org/wikipedia/commons/c/c0/Carolus_Linnaeus.jpg" alt="Portrait of Carl von Linné (Carolus Linnaeus) - Public domain" width="300"/>

As mentioned in our [FAQ](https://www.gbif.org/faq?question=how-can-i-publish-molecular-data-to-gbif):
> A basic requirement [on GBIF] is that organisms are identified–either using Linnean classification or another accepted taxonomy (e.g. DNA-based Species Hypotheses (SH) for fungi). If a specific taxonomy is not available, organisms may be identified to the nearest possible taxon (e.g. Bacteria).

In other words, checklists, occurrence and sampling-event datasets must contain scientific names (our system doesn't handle OTUs). If you would like to check what can be matched to the GBIF taxonomy, please try out our name matching tool: https://www.gbif.org/tools/species-lookup.

The [BIOWIDE eDNA Fungi dataset](https://doi.org/10.15468/nesbvx) is a good example of dataset with some DNA-based Species Hypotheses (SH) for fungi.
If no scientific name is available, you can always publish metadata-only datasets. See for example this dataset about [Microbial Fungi in soils on different Sub-Antarctic islands](https://doi.org/10.15468/jekfdj).


# Protocols and analysis pipelines

The sampling protocol, sequencing technology, quality control and sequence analysis pipelines make a huge difference in what can be detected at the microscopic level. For metagenomics datasets, the set of methods employed is probably the most valuable piece of information for a data user.
There are two ways to convey this type of information:

* Using extensions
* **Not** using extensions

In the first case, the information can be structured using defined terms, which makes it easier for users to compare methods once the data is aggregated with other dataset. In addition to that, this is a good way to make the information compatible across platforms. For example, the [BIOWIDE eDNA Fungi dataset](https://doi.org/10.15468/nesbvx) uses [GGNB extensions](http://rs.gbif.org/extension/ggbn/). However, GBIF doesn't index extensions. Which means that the information isn't displayed on the portal. In fact, it is only available to users downloading the raw Darwin Core archive (see screenshot below).

<img src="/2019-04-23-gbif-molecular-data-publishing_images/raw_DwcA_download.png" alt="Download Raw DwC-A" width="600"/>

The alternative is to use some of the [Darwin Core terms](https://dwc.tdwg.org/terms/). For example, [Mgnify](https://www.gbif.org/publisher/ab733144-7043-4e88-bd4f-fca7bf858880) organized its datasets as follow:

* The sampling protocols are described in the event files under the [dwc:samplingProtocol](https://dwc.tdwg.org/terms/#dwc:samplingProtocol) and the [dwc:eventRemarks](https://dwc.tdwg.org/terms/#dwc:eventRemarks) terms (see [this event](https://www.gbif.org/dataset/20b968fd-b7e0-4002-9136-1ccfa8e5b9df/event/MGYA00163069) from the [Inter-Comparison of Marine Plankton Metagenome Analysis Methods](https://doi.org/10.15468/re7eoi)).
* The bioinformatics analysis pipelines are described in the occurrence files under [dwc:identificationReferences](https://dwc.tdwg.org/terms/#dwc:identificationReferences) with a link to the pipeline and [dwc:identificationRemarks](https://dwc.tdwg.org/terms/#dwc:identificationRemarks) with the description (see [this occurrence](https://www.gbif.org/occurrence/2027240778) from the same dataset).
* The total number of reads in a sample are given in the event file under [dwc:sampleSizeValue](https://dwc.tdwg.org/terms/#dwc:sampleSizeValue) and [dwc:sampleSizeUnit](https://dwc.tdwg.org/terms/#dwc:sampleSizeUnit) and the number of reads matched to a specific taxon are given in the occurrence file under [dwc:organismQuantity](https://dwc.tdwg.org/terms/#dwc:organismQuantity) and [dwc:organismQuantityType](https://dwc.tdwg.org/terms/#dwc:organismQuantityType).

These fields are displayed on the occurrence and event pages but contain mostly free text. The lack of structure can be an issue when comparing data from different data sources.

Note that the methods can also be described in the datasets metadata. For example, the [SCAR - Microbial Antarctic Resource System metadata-only datasets](https://www.gbif.org/dataset/search?type=METADATA&publishing_org=af290483-8639-4b58-87fb-a4824c65e577) contain sampling and laboratory protocols (see for example [Microbial Fungi in soils on different Sub-Antarctic islands](https://doi.org/10.15468/jekfdj)) 

A third option could be to do both: structure the information in extensions and make it available in [Darwin Core terms](https://dwc.tdwg.org/terms/). This approach would require more work but would ensure that the information is structured and accessible to more users.


## Extensions available

A few extensions are currently available for structuring laboratory protocols. As mentioned previously, [BIOWIDE eDNA Fungi dataset](https://doi.org/10.15468/nesbvx) uses [GGNB extensions](http://rs.gbif.org/extension/ggbn/).
The [MIxS sample extension](https://rs.gbif.org/sandbox/extension/mixs_sample.xml) can also be an alternative.

Keep in mind that GBIF doesn't maintain extensions (it is up to the community) so some extensions available can become deprecated.

# Sequences

You cannot make your sequences available directly on GBIF but you can reference them via the [dwc:associatedSequences](https://dwc.tdwg.org/terms/#dwc:associatedSequences) field or share them via an extension.

The [dwc:associatedSequences](https://dwc.tdwg.org/terms/#dwc:associatedSequences) field should contain a reference to a sequence (for example to EBI or EMBL) not the sequence itself, see for example, the [Centre for Biodiversity Genomics - Canadian Specimens](https://doi.org/10.15468/mbwnw9).

The [GGNB extensions](http://rs.gbif.org/extension/ggbn/) allow to share sequences but it is not indexed by the system and is therefore available only in the raw Darwin Core archive, see the [BIOWIDE eDNA Fungi dataset](https://doi.org/10.15468/nesbvx).

# Environmental data

Your samples or observations might be associated with measurements such as the water salinity or the soil pH. You can structure this type of information by using the [MeasurmentOrFact](http://rs.tdwg.org/dwc/terms/#measurementorfact) extension. However, as I mentioned in the two paragraphs above, GBIF doesn't index extension so these measurements would only be available in the Darwin Core Archive.

An alternative would be to use the [dwc:dynamicProperties](http://rs.tdwg.org/dwc/terms/#dwc:dynamicProperties) field. This solution is not ideal but the properties will be displayed on the portal (see [this occurrence](https://www.gbif.org/occurrence/2027240778) from the [Inter-Comparison of Marine Plankton Metagenome Analysis Methods](https://doi.org/10.15468/re7eoi)).

Perhaps the best solution for now would be to use both [MeasurmentOrFact](http://rs.tdwg.org/dwc/terms/#measurementorfact) and [dwc:dynamicProperties](http://rs.tdwg.org/dwc/terms/#dwc:dynamicProperties) to ensure that the information is accessible.

# In summary

Publishing molecular data on GBIF is like publishing any other type of data, it depends on each particular case. We are still figuring things out and would appreciated any thoughts on the topic. Don't hesitate to leave a comment or email helpdesk@gbif.org if you have any question.

# References

* [Adding sequence-based identifiers to backbone taxonomy reveals 'dark taxa' fungi](https://www.gbif.org/news/2LrgV5t3ZuGeU2WIymSEuk/adding-sequence-based-identifiers-to-backbone-taxonomy-reveals-dark-taxa-fungi)
* [UNITE](https://unite.ut.ee)
* [Biodiversity infrastructures to crosslink metagenomics and species occurrence data](https://www.gbif.org/news/6ewyUhBpRYammYWI2CgsM4/biodiversity-infrastructures-to-crosslink-metagenomics-and-species-occurrence-data)
* [EBI](https://www.ebi.ac.uk)
* [(How) can I publish molecular/sequence/DNA based data to GBIF?](https://www.gbif.org/faq?question=how-can-i-publish-molecular-data-to-gbif)
* [Dataset classes](https://www.gbif.org/dataset-classes)
* [Darwin Core Archives - How-to Guide](https://github.com/gbif/ipt/wiki/DwCAHowToGuide)
* [Darwin Core terms quick reference guide](https://dwc.tdwg.org/terms/)
* [IPT manual](https://github.com/gbif/ipt/wiki)
* [Quick guide to publishing data through GBIF.org](https://www.gbif.org/publishing-data)
* [FAQ - (How) can I publish molecular/sequence/DNA based data to GBIF?](https://www.gbif.org/faq?question=how-can-i-publish-molecular-data-to-gbif)
* [Name matching tool](https://www.gbif.org/tools/species-lookup)
* [Extensions](https://tools.gbif.org/dwca-validator/extensions.do)
* [MeasurmentOrFact extension](http://rs.tdwg.org/dwc/terms/#measurementorfact)
* [GGNB extensions](http://rs.gbif.org/extension/ggbn/)

* Frøslev T, Ejrnæs R (2018). BIOWIDE eDNA Fungi dataset. Danish Biodiversity Information Facility. Occurrence dataset https://doi.org/10.15468/nesbvx accessed via GBIF.org on 2019-04-17.
* PlutoF (2019). UNITE - Unified system for the DNA based fungal species linked to the classification. Version 1.2. Checklist dataset https://doi.org/10.15156/bio/587474 accessed via GBIF.org on 2019-04-17.
* Cox F, Newsham K, Robinson C, Sweetlove M (2019). Microbial Fungi in soils on different Sub-Antarctic islands. SCAR - Microbial Antarctic Resource System. Metadata dataset https://doi.org/10.15468/jekfdj accessed via GBIF.org on 2019-04-17. + [all the SCAR - Microbial Antarctic Resource System metadata-only datasets](https://www.gbif.org/dataset/search?type=METADATA&publishing_org=af290483-8639-4b58-87fb-a4824c65e577)
* MGnify (2017). EMOSE (2017) Inter-Comparison of Marine Plankton Metagenome Analysis Methods. Sampling event dataset https://doi.org/10.15468/re7eoi accessed via GBIF.org on 2019-04-17. + [MGnify datasets](https://www.gbif.org/dataset/search?publishing_org=ab733144-7043-4e88-bd4f-fca7bf858880)
* Telfer A (2018). Centre for Biodiversity Genomics - Canadian Specimens. University of Guelph. Occurrence dataset https://doi.org/10.15468/mbwnw9 accessed via GBIF.org on 2019-04-17.
