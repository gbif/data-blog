---
title: GBIF Issues & Flags
author: Leonardo Buitrago
date: '2020-10-27'
slug: issues-and-flags
categories:
  - GBIF
tags:

  - GBIF
  - issues
  - flags
  - data quality

lastmod: '2020-10-08'
draft: no
keywords: ['issues', 'flags', 'quality', 'interpretation', 'users', 'publishers', 'GBIF']
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

**Publishers** share datasets, but also manage data quality.  **GBIF** provides **access** to the use of biodiversity data, but also **flags** suspicious or missing content. **Users** use data, but also clean and remove records. Each play an important role in managing and improving data quality..

<!--more-->

![](/post/2020-10-09-issues-and-flags_files/workflow1.png)

## What are GBIF issues and flags?

The GBIF network publishes datasets, integrating them into a common access system. Here users can retrieve data through common search and download services. During the indexation process over the raw data, GBIF adds **issues** and **flags** to records with common data quality problems.

<!--
During **interpretation** GBIF performs additional checks and conversion routines. This **interpretation** is to ensure that data are interoperable and useful.

-->

![](/post/2020-10-09-issues-and-flags_files/workflow2.png)

<!--

* Flags help users to filter or be aware of possible inconsistencies in the data.
* Flags help data providers detect different quality issues that can be fixed in the published dataset/records.

-->

<!--

![](/post/2020-10-09-issues-and-flags_files/issues&flags_main.png)

 Write your comments here 

Thinking on how publishers and users can deal with the issues/flags identified by GBIF.org is important to recognize the most common problems that this mechanism can help to improve the data quality:

- Lack in the use of controlled vocabularies (e.g. basisOfRecord)
- Inconsistent data (e.g. country ≠ countryCode)
- Using appropriate standardized formats, like ISO codes (e.g. countryCode, eventDate)
- Using valid values to interpret/index numeric data (e.g. individualCount, coordinateUncertaintyInMeters)
- Values between the correct numeric range laid in the right columns (e.g. elevation/depth swapped)
- Inappropriate use of zero (0) to document empty or missing values (e.g. decimal Lat/Long = 0)
- Using correct characters to build lists (e.g. references, url)

-->

**Excluding all records** with a particular issue is **not** currently possible with the search interface. It is possible to filter all records you are not interested in **with issues** by selecting the particular issue and hitting the **reverse button**. However, reversing will still only give you all other flagged occurrences and **not** issue-free records. This is something that GBIF is working to improve. (at [occurrence search](https://www.gbif.org/occurrence/search))

![](/post/2020-10-09-issues-and-flags_files/filtering.png)

**Remarks** are shown on the individual occurrence pages to explain the process done after interpretation:

1. **Excluded** means the original data couldn’t be interpreted, so is excluded in the interpreted fields.
2. **Altered** means the original data is modified in the interpretation process to be indexed in GBIF.org.
3. **Inferred** means the Using other record information the data indexed is inferred, if the original is empty.

![](/post/2020-10-09-issues-and-flags_files/excluded.png)

![](/post/2020-10-09-issues-and-flags_files/altered.png)

![](/post/2020-10-09-issues-and-flags_files/inferred.png)

<!--

### For Data providers (“publishers”)

A list of issues & flags is also available for each dataset at the dataset page metrics (e.g. https://www.gbif.org/dataset/4fa7b334-ce0d-4e88-aaae-2e0c138d049e/metrics). This helps publishers to identify what data quality issues to tackle.

![](/post/2020-10-09-issues-and-flags_files/dataset.png)

## How to improve data quality using issues and flags?

-->

The following table highlights some common geospatial issues and instructs how to fix them. 

<style>
#issues {
  font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
  max-width: 100%;
  overflow: auto;
}

#issues td, #issues th {
  border: 1px solid #ddd;
  padding: 8px;
}

#issues tr:nth-child(even){background-color: #f2f2f2;}

#issues tr:hover {background-color: #ddd;}

#issues th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #4B9E46;
  color: white;
}

</style>

<table id = "issues">
<thead>
<tr>
<th align="left"><strong>Issue &amp; flag</strong></th>
<th align="left"><strong>Action to take</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Country derived from coordinates</td>
<td align="left">Fill in the columns <strong>countryCode</strong> and <strong>country</strong> with the country information where the record was registered, following the officially ISO 3166-1-alpha-2 country code</td>
</tr>
<tr>
<td align="left">Recorded date invalid</td>
<td align="left">Use existing valid dates in the columns <strong>eventDate</strong>, <strong>year</strong>, <strong>month</strong>, <strong>day</strong>, following the format ISO 8601-1:2019 (YYYY-MM-DD)</td>
</tr>
<tr>
<td align="left">Basis of record invalid</td>
<td align="left">Use a valid Basis of Record in the column <strong>basisOfRecord</strong>, according to the nature of the record. Follow the controlled vocabulary present on this list <a href="https://rs.gbif.org/vocabulary/dwc/basis_of_record.xml">https://rs.gbif.org/vocabulary/dwc/basis_of_record.xml</a></td>
</tr>
<tr>
<td align="left">Country coordinate mismatch</td>
<td align="left">Make sure coordinates (<strong>decimalLatitude</strong><strong>decimalLongitude</strong>), fall inside the indicated country (i.e. country countryCode). The country and countryCode must match and be documented following the officially ISO 3166-1-alpha-2.</td>
</tr>
<tr>
<td align="left">Zero coordinate</td>
<td align="left">Leave <strong>decimalLatitude</strong> and <strong>decimalLongitude</strong> blank if the coordinates are missing. Don't use "0" as a coordinate value in both columns unless your record be present there (Null Island <a href="https://en.wikipedia.org/wiki/Null_Island">https://en.wikipedia.org/wiki/Null_Island</a>).</td>
</tr>
<tr>
<td align="left">Coordinate invalid</td>
<td align="left">Make sure coordinates are valid numeric decimal values. <strong>decimalLatitude</strong>: legal values lie between -90 and 90, inclusive. and <strong>decimalLongitude</strong>: legal values lie between -180 and 180, inclusive. Also, <strong>verbatimCoordinates</strong> have to be valid values for coordinates in decimal degrees, degrees decimal minutes, degrees minutes second</td>
</tr>
</tbody>
</table>


<br>
<br>
## Definitions

More than **50 issues and flags** have been created to deal with common data quality problems. The following **long** section compiles all of them and offers a more clear description of each one. This section is intended to serve as a placeholder until more formal documentation can be written.  

<big>**Geospatial Issues**</big>

- - -

**Zero coordinate** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=ZERO_COORDINATE)</small><br>Coordinates are exactly 0/0, often indicating an actual null coordinate.<br><small>**Terms**: dwc:decimalLatitude, dwc:decimalLongitude</small><br>

**Country coordinate mismatch** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COUNTRY_COORDINATE_MISMATCH)</small><br>The interpreted occurrence coordinates fall outside of the indicated country.<br><small>**Terms**: dwc:countryCode, dwc:country, dwc:decimalLatitude, dwc:decimalLongitude</small><br>

**Coordinate invalid** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COORDINATE_INVALID)</small><br>A coordinate value is given in some form, but GBIF is unable to interpret it. Possible reasons include, i.a., coordinates that fall out of range (larger/lower than 90/-90 or 180/-180, depending) or text values that cannot be interpreted.<br><small>**Terms**: dwc:decimalLatitude, dwc:decimalLongitude, dwc:verbatimCoordinates, dwc:verbatimLatitude, dwc:verbatimLongitude</small><br>

**Coordinate out of range** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COORDINATE_OUT_OF_RANGE)</small><br>The supplied coordinates lie outside of the range for decimal lat/lon values (-90/90, -180/180).<br><small>**Terms**: dwc:decimalLatitude, dwc:decimalLongitude, dwc:verbatimCoordinates, dwc:verbatimLatitude, dwc:verbatimLongitude</small><br>

These 4 issues are removed by default when **including coordinates** and **not** clicking the check box: 

<img src="/post/2020-10-09-issues-and-flags_files/suspicious.png" alt="HTML tutorial" style="width:50%;">

- - -

**Geodetic datum assumed WGS84** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=GEODETIC_DATUM_ASSUMED_WGS84)</small><br>If the datum is null, data interpretation assumes the record coordinates are in WGS84.<br><small>**Terms**: dwc:geodeticDatum</small><br>

**Geodetic datum invalid** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=GEODETIC_DATUM_INVALID)</small><br>The geodetic datum could not be interpreted, because the supplied term cannot be matched against the vocabulary of known values.<br><small>**Terms**: dwc:geodeticDatum</small><br>

**Country mismatch** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COUNTRY_MISMATCH)</small><br>Interpreted Country and Country code contradict each other.<br><small>**Terms**: dwc:countryCode, dwc:country</small><br>

**Country derived from coordinates** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COUNTRY_DERIVED_FROM_COORDINATES)</small><br>If the country and country code are not supplied or cannot be matched to known values, data interpretation derives their content from the decimal coordinates through a [lookup service](https://github.com/gbif/geocode).<br><small>**Terms**: dwc:countryCode, dwc:country, dwc:decimalLatitude, dwc:decimalLongitude</small><br>

**Country invalid** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COUNTRY_INVALID)</small><br>The country or countryCode given cannot be matched to the vocabulary for country names.<br><small>**Terms**: dwc:country</small><br>

**Continent invalid** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=CONTINENT_INVALID)</small><br>The continent given cannot be matched to the vocabulary for continent names<br><small>**Terms**: dwc:continent</small><br>

**Coordinate rounded** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COORDINATE_ROUNDED)</small><br>In the data interpretation the original coordinates are rounded to 6 decimals (~1m precision).<br><small>**Terms**: dwc:decimalLatitude, dwc:decimalLongitude</small><br>

**Coordinate reprojected** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COORDINATE_REPROJECTED)</small><br>The original coordinates were successfully reprojected from a different geodetic datum to WGS84.<br><small>**Terms**: dwc:geodeticDatum</small><br>

**Coordinate reprojection suspicious** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COORDINATE_REPROJECTION_SUSPICIOUS)</small><br>Indicates successful coordinate reprojection according to provided datum, but which results in a datum shift larger than 0.1 decimal degrees.<br><small>**Terms**: dwc:geodeticDatum, dwc:decimalLatitude, dwc:decimalLongitude</small><br>

**Coordinate reprojection failed** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COORDINATE_REPROJECTION_FAILED)</small><br>The given decimal latitude and longitude could not be reprojected to WGS84 based on the provided datum.<br><small>**Terms**: dwc:geodeticDatum, dwc:decimalLatitude, dwc:decimalLongitude</small><br>

**Coordinate uncertainty meters invalid** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COORDINATE_UNCERTAINTY_METERS_INVALID)</small><br>The value given for Coordinate uncertainty in meters, indicating the radius of uncertainty around the given decimal coordinates, is not a valid number, or lies outside a plausible range.<br><small>**Terms**: dwc:coordinateUncertaintyInMeters</small><br>

**Coordinate precision invalid** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COORDINATE_PRECISION_INVALID)</small><br>Indicates an invalid or very unlikely coordinates precision. The value is not a decimal number as expected, or it has an unusually low or high for a margin of uncertainty.<br><small>**Terms**: dwc:coordinatePrecision</small><br>

**Presumed negated longitude** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=PRESUMED_NEGATED_LONGITUDE)</small><br>The supplied longitude value places the coordinates outside of the indicated country. Negating the longitude value would result in a country match.<br><small>**Terms**: dwc:decimalLongitude</small><br>

**Presumed negated latitude** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=PRESUMED_NEGATED_LATITUDE)</small><br>The supplied latitude value places the coordinates outside of the indicated country. Negating the latitude value would result in a country match.<br><small>**Terms**: dwc:decimalLatitude</small><br>

**Presumed swapped coordinate** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=PRESUMED_SWAPPED_COORDINATE)</small><br>Coordinates seem to be swapped when testing against the interpreted country.<br><small>**Terms**: dwc:decimalLatitude, dwc:decimalLongitude, dwc:country</small><br>

**Depth min max swapped** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=DEPTH_MIN_MAX_SWAPPED)</small><br>The values for minimum and maximum depth appear to the swapped.<br><small>**Terms**: dwc:minimumDepthInMeters, dwc:maximumDepthInMeters</small><br>

**Depth non numeric** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=DEPTH_NON_NUMERIC)</small><br>The values for minimum and maximum depth are non-numeric values and cannot be interpreted.<br><small>**Terms**: dwc:minimumDepthInMeters, dwc:maximumDepthInMeters</small><br>

**Depth unlikely** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=DEPTH_UNLIKELY)</small><br>The values for minimum and maximum depth are negative or higher than 11000 (Mariana Trench depth in meters).<br><small>**Terms**: dwc:minimumDepthInMeters, dwc:maximumDepthInMeters</small><br>

**Depth not metric** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=DEPTH_NOT_METRIC)</small><br>Set if supplied depth is not given in the metric system, for example using feet instead of meters.<br><small>**Terms**: dwc:minimumDepthInMeters, dwc:maximumDepthInMeters</small><br>

**Elevation non numeric** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=ELEVATION_NON_NUMERIC)</small><br>The values for minimum and maximum elevation are non-numeric values and cannot be interpreted.<br><small>**Terms**: dwc:minimumElevationInMeters, dwc:maximumElevationMeters</small><br>

**Elevation min max swapped** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=ELEVATION_MIN_MAX_SWAPPED)</small><br>The values for minimum and maximum elevation appear to the swapped.<br><small>**Terms**: dwc:minimumElevationInMeters, dwc:maximumElevationInMeters</small><br>

**Elevation not metric** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=ELEVATION_NOT_METRIC)</small><br>Set if supplied elevation is not given in the metric system, for example using feet instead of meters.<br><small>**Terms**: dwc:minimumElevationInMeters, dwc:maximumElevationInMeters</small><br>

- - -

**Zero** occurrence records are flagged with the following **geospatial issues** on GBIF as of the writing of this post. 

**Elevation unlikely** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=ELEVATION_UNLIKELY)</small><br>The values for minimum and maximum elevation are above the troposphere (17000 m) or below Mariana Trench (11000 m).<br><small>**Terms**: dwc:minimumElevationInMeters, dwc:maximumElevationInMeters</small><br>

**Continent country mismatch** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=CONTINENT_COUNTRY_MISMATCH)</small><br>The interpreted continent and country do not match up.<br><small>**Terms**: dwc:continent, dwc:countryCode, dwc:country</small><br>

**Continent derived from coordinates** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=CONTINENT_DERIVED_FROM_COORDINATES)</small><br>If no value is supplied for the continent or if the values cannot be matched against a known vocabulary, data interpretation derives the continent from the decimal coordinates.<br><small>**Terms**: dwc:continent, dwc:decimalLatitude, dwc:decimal Longitude</small><br>

<!-- maybe do not we need deprecated issues -->
<!-- **Coordinate accuracy invalid** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COORDINATE_ACCURACY_INVALID)</small><br>Deprecated.<br><small>**Terms**: NA</small><br> -->

<!-- **Coordinate precision uncertainty mismatch** <small>(geospatial)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COORDINATE_PRECISION_UNCERTAINTY_MISMATCH)</small><br>Deprecated.<br><small>**Terms**: NA</small><br> -->

<br>
<big>**Taxonomic Issues**</big>

- - -

**Taxon match higherrank** <small>(taxonomic)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=TAXON_MATCH_HIGHERRANK)</small><br>The record can be matched to the GBIF taxonomic backbone at a higher rank, but not with the scientific name given.<br><small>**Terms**: dwc:scientificName,dwc:kingdom,dwc:phylum, dwc:class, dwc:order, dwc:family, dwc:genus, dwc:subgenus, dwc:specificEpithet, dwc:infraspecificEpithet, dwc:taxonRank</small><br><br> 
Reasons include:<br/>- The name is new, and not available in the taxonomic datasets yet<br/>- The name is missing in the backbone's taxonomic sources for others reasons<br/>- Formatting or spelling of the scientific name caused interpretation errors

**Taxon match none** <small>(taxonomic)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=TAXON_MATCH_NONE)</small><br>Matching to the taxonomic backbone cannot be done cause there was no match at all or several matches with too little information to keep them apart (homonyms).<br><small>**Terms**: dwc:scientificName,dwc:kingdom,dwc:phylum, dwc:class, dwc:order, dwc:family, dwc:genus, dwc:subgenus, dwc:specificEpithet, dwc:infraspecificEpithet, dwc:taxonRank</small><br>

**Taxon match fuzzy** <small>(taxonomic)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=TAXON_MATCH_FUZZY)</small><br>Matching to the taxonomic backbone can only be done using a fuzzy, non exact match.<br><small>**Terms**: dwc:scientificName,dwc:kingdom,dwc:phylum, dwc:class, dwc:order, dwc:family, dwc:genus, dwc:subgenus, dwc:specificEpithet, dwc:infraspecificEpithet, dwc:taxonRank</small><br>

<br>
<big>**Date Issues**</big>

- - -

**Recorded date invalid** <small>(date)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=RECORDED_DATE_INVALID)</small><br>The recording date given cannot be intrepreted because is invalid.<br><small>**Terms**: dwc:eventDate, dwc:year, dwc:month, dwc:day</small><br><br> Reasons include:<br/>- A non-existing date (e.g "1995-04-34")<br/>- Missing date parts (e.g. Event date without year).<br/>- The date format does not follow the ISO 8601 standard (YYYY-MM-DD)<br>

**Recorded date mismatch** <small>(date)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=RECORDED_DATE_MISMATCH)</small><br>The recording date specified as the eventDate string and the individual year, month, day are contradicting.<br><small>**Terms**: dwc:eventDate, dwc:year, dwc:month, dwc:day</small><br>

**Identified date unlikely** <small>(date)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=IDENTIFIED_DATE_UNLIKELY)</small><br>The identification date is in the future or before Linnean times (1700).<br><small>**Terms**: dwc:dateIdentified</small><br>

**Recorded Date Unlikely** <small>(date)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=RECORDED_DATE_UNLIKELY)</small><br>The recording date is highly unlikely, falling either into the future or representing a very old date before 1600 that predates modern taxonomy.<br><small>**Terms**: dwc:eventDate, dwc:year, dwc:month, dwc:day</small><br>

**Multimedia date invalid** <small>(date)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=MULTIMEDIA_DATE_INVALID)</small><br>The creation date given cannot be intrepreted because is invalid.<br><small>**Terms**: dc:created</small><br><br> Reasons include:<br/>- A non-existing date (e.g "1995-04-34")<br/>- Missing date parts (e.g. Event date without year).<br/>- The date format does not follow the ISO 8601 standard (YYYY-MM-DD)

**Identified date invalid** <small>(date)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=IDENTIFIED_DATE_INVALID)</small><br>The identification date given cannot be intrepreted because is invalid.<br><small>**Terms**: dwc:dateIdentified</small><br><br>Reasons include:<br>- A non-existing date (e.g "1995-04-34")<br>- Missing date parts (e.g. without year).<br>- The date format does not follow the ISO 8601 standard (YYYY-MM-DD)<br>

**Modified date invalid** <small>(date)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=MODIFIED_DATE_INVALID)</small><br>A (partial) invalid modified date is given.<br><small>**Terms**: dc:modified</small><br><br>Reasons include:<br/>- A non-existing date (e.g "1995-04-34")<br/>- Missing date parts (e.g. without year).<br/>- The date format does not follow the ISO 8601 standard (YYYY-MM-DD)

**Modified date unlikely** <small>(date)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=MODIFIED_DATE_UNLIKELY)</small><br>The modified date given is in the future or predates unix time (1970).<br><small>**Terms**: dc:modified</small><br>

**Georeferenced date invalid** <small>(date)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=GEOREFERENCED_DATE_INVALID)</small><br>The georeference date given cannot be intrepreted because it is invalid.<br> 
<small>**Terms**: dwc:georeferencedDate</small><br>

Reasons include:<br>
- A non-existing date (e.g "1995-04-34").<br>
- Missing date parts (e.g. without year).<br>
- The date format does not follow the ISO 8601 standard (YYYY-MM-DD)

**Georeferenced date unlikely** <small>(date)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=GEOREFERENCED_DATE_UNLIKELY)</small><br>The georeference date given is in the future or before Linnean times (1700).<br><small>**Terms**: dwc:georeferencedDate</small><br>

<br>
<big>**Vocabulary Issues**</big>

- - -

**Basis of record invalid** <small>(vocabulary)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=BASIS_OF_RECORD_INVALID)</small><br>The given basis of record is impossible to interpret or very different from the recommended vocabulary: http://rs.gbif.org/vocabulary/dwc/basis_of_record.xml<br><small>**Terms**: dwc:basisOfRecord</small><br>

**Type status invalid** <small>(vocabulary)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=TYPE_STATUS_INVALID)</small><br>The given type status is impossible to interpret or very different from the recommended vocabulary: https://rs.gbif.org/vocabulary/gbif/type_status.xml<br><small>**Terms**: dwc:typeStatus</small><br>

**Occurrence status unparsable** <small>(vocabulary)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=OCCURRENCE_STATUS_UNPARSABLE)</small><br>The given occurenceStatus value cannot be interpreted; it does not match any of the known (vocabulary) values that indicate the presence or absence of a species at collection or observation event.<br><small>**Terms**: dwc:occurrenceStatus</small><br>

<br>
<big>**GRSciColl-related Issues**</big>

- - -

**Ambiguous institution** <small>(GRSciColl)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=AMBIGUOUS_INSTITUTION)</small><br>Multiple institutions were found in [GRSciColl](https://www.gbif.org/grscicoll) with the same level of confidence and it can't be determined which one should be accepted. For example, there are several institutions with the same code and country. See [this FAQ](https://www.gbif.org/faq?question=how-can-i-improve-the-matching-of-occurrence-records-with-grscicoll) on how to avoid ambiguous matches.<br><small>**Terms**: dwc:institutionCode, dwc:institutionID</small><br>


**Ambiguous collection** <small>(GRSciColl)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=AMBIGUOUS_COLLECTION)</small><br>Multiple collections were found in [GRSciColl](https://www.gbif.org/grscicoll) with the same level of confidence and it can't be determined which one should be accepted. For example, there are several collections belonging to the same institution with the same code. See [this FAQ](https://www.gbif.org/faq?question=how-can-i-improve-the-matching-of-occurrence-records-with-grscicoll) on how to avoid ambiguous matches.<br><small>**Terms**: dwc:collectionCode, dwc:collectionID</small><br>


**Institution match none** <small>(GRSciColl)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=INSTITUTION_MATCH_NONE)</small><br>No macth was  found in [GRSciColl](https://www.gbif.org/grscicoll). Either the entry doesn't exists in GRSciColl or it has a different code. Check [GRSciColl](https://www.gbif.org/grscicoll) and request update if needed.<br><small>**Terms**: dwc:institutionCode, dwc:institutionID</small><br>

**Collection match none** <small>(GRSciColl)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COLLECTION_MATCH_NONE)</small><br>No macth was  found in [GRSciColl](https://www.gbif.org/grscicoll). Either the entry doesn't exists in GRSciColl or it has a different code. Check [GRSciColl](https://www.gbif.org/grscicoll) and request update if needed.<br><small>**Terms**: dwc:collectionCode, dwc:collectionID</small><br>

**Institution match fuzzy** <small>(GRSciColl)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=INSTITUTION_MATCH_FUZZY)</small><br>A match was found in [GRSciColl](https://www.gbif.org/grscicoll) but it was matched fuzzily. To know more about why this has happened you can use the [lookup API](https://www.gbif.org/developer/registry#lookup) to see see the "reasons" returned in the response. A common case is when the name is used instead of the code or the identifier. To avoid fuzzy matches, publishers should use identifiers in additon to codes. More details available in [this FAQ](https://www.gbif.org/faq?question=how-can-i-improve-the-matching-of-occurrence-records-with-grscicoll).<br><small>**Terms**: dwc:institutionCode, dwc:institutionID</small><br>

**Collection match fuzzy** <small>(GRSciColl)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=COLLECTION_MATCH_FUZZY)</small><br>A match was found in [GRSciColl](https://www.gbif.org/grscicoll) but it was matched fuzzily. To know more about why this has happened you can use the [lookup API](https://www.gbif.org/developer/registry#lookup) to see see the "reasons" returned in the response. A common case is when the name is used instead of the code or the identifier. To avoid fuzzy matches, publishers should use identifiers in additon to codes. More details available in [this FAQ](https://www.gbif.org/faq?question=how-can-i-improve-the-matching-of-occurrence-records-with-grscicoll).<br><small>**Terms**: dwc:collectionCode, dwc:collectionID</small><br>

**Institution collection mismatch** <small>(GRSciColl)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=INSTITUTION_COLLECTION_MISMATCH)</small><br>At least one possible collection match was found in [GRSciColl](https://www.gbif.org/grscicoll) but none of them belong to the institution matched.<br><small>**Terms**: dwc:collectionCode, dwc:collectionID, dwc:institutionCode, dwc:institutionID</small><br>

**Different owner institution** <small>(GRSciColl)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=DIFFERENT_OWNER_INSTITUTION)</small><br>The institution doesn't match the owner institution.<br><small>**Terms**: dwc:ownerInstitutionCode, dwc:institutionCode, dwc:institutionID</small><br>


<br>
<big>**Other Issues**</big>

- - -

**Individual count invalid** <small>(individual count)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=INDIVIDUAL_COUNT_INVALID)</small><br>Individual count value not parsable into a positive integer.<br><small>**Terms**: dwc:individualCount</small><br>

**Individual count conflicts with occurrence status** <small>(individual count)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=INDIVIDUAL_COUNT_CONFLICTS_WITH_OCCURRENCE_STATUS)</small><br>The values given for the individual count and for the status of the occurrence (present/absent) contradict each other (e.g. the count is 0 but the status says "present").<br><small>**Terms**: dwc:individualCount, dwc:occurrenceStatus</small><br>

**Occurrence status inferred from individual count** <small>(occurrence status)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=OCCURRENCE_STATUS_INFERRED_FROM_INDIVIDUAL_COUNT)</small><br>The present/absent status of the occurrence was inferred from the individual count value because no status value was supplied explicitly. An individual count of 0 is interpreted as status="absent", a value > 0 as "present"<br><small>**Terms**: dwc:individualCount, dwc:occurrenceStatus</small><br>

**References URI invalid** <small>(uri)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=REFERENCES_URI_INVALID)</small><br>The references URL cannot be resolved, and may be malformed or contain invalid characters. If there is more than one URL, the values have to be separated by a pipe symbol "|".<br><small>**Terms**: dc:references</small><br>

**Multimedia URI invalid** <small>(uri)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=MULTIMEDIA_URI_INVALID)</small><br>The multimedia URL cannot be resolved, and may be malformed or contain invalid characters. If there is more than one URL, the values have to be separated by a pipe symbol "|".<br><small>**Terms**: dwc:associatedMedia</small><br>

**Interpretation error** <small>(interpretation)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=INTERPRETATION_ERROR)</small><br>An error occurred during interpretation, leaving the record interpretation incomplete.<br><small>**Terms**: GBIF interpretation</small><br>

**Occurrence status inferred from Basis Of Record**  <small>(interpretation)</small> <small>[example](https://www.gbif.org/occurrence/search?issue=OCCURRENCE_STATUS_INFERRED_FROM_BASIS_OF_RECORD)</small>
<br>The occurrence status of preserved specimens (museum specimens) is assumed to be "present" (not absent). With other records occurrence status is usually inferred from from dwc:individualCount.<br><small>**Terms**: GBIF interpretation,  dwc:individualCount</small><br>





