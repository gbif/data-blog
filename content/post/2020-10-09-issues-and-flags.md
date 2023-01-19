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


## Checklists issues and flags [Edit from 2023-01-19]

The GBIF system also flags taxon records coming from checklists. You can search and select checklist records by flags in our [Species search interface](https://www.gbif.org/species/search?advanced=1):

![](/post/2020-10-09-issues-and-flags_files/checklist_flags.png)

See the definitions below:


**Name unparsable** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=UNPARSABLE&advanced=1)</small><br>The value in the field flagged couldn't be parsed by the GBIF system. You can check if a scientific name can be parsed with our name parser tool: https://www.gbif.org/tools/name-parser <br><small>**Terms**: any name field</small><br>


**Name partially parsed** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=PARTIALLY_PARSABLE&advanced=1)</small><br>The value in the field flagged could only be partially parsed by the GBIF system. You can check if a scientific name can be parsed with our name parser tool: https://www.gbif.org/tools/name-parser <br><small>**Terms**: any name field</small><br>


**ParentNameUsageID invalid** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=PARENT_NAME_USAGE_ID_INVALID&advanced=1)</small><br>The value for the ParentNameUsageID doesn't correspond to a valid entry in the list. Check that the parentNameUsageID points to an existing taxon entry within the checklist (parentNameUsageID should contain the value of the taxonID of the parent taxon in the same checklist)<br><small>**Terms**: [dwc:parentNameUsageID](https://dwc.tdwg.org/list/#dwc_parentNameUsageID)</small><br>


**AcceptedNameUsageID invalid** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=ACCEPTED_NAME_USAGE_ID_INVALID&advanced=1)</small><br>The value for the acceptedNameUsageID could not be resolved. Check that the acceptedNameUsageID points to an existing taxon entry within the checklist (for synonyms or misapplied named, acceptedNameUsageID should contain the value of the taxonID of the accepted/valid taxon name in the checklist)<br><small>**Terms**: [dwc:acceptedNameUsageID](https://dwc.tdwg.org/list/#dwc_acceptedNameUsageID)</small><br>


**OriginalNameUsageID invalid** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=ORIGINAL_NAME_USAGE_ID_INVALID&advanced=1)</small><br>The value for originalNameUsageID could not be resolved. Check that the originalNameUsageID points to an existing taxon entry within the checklist (originalNameUsageID should contain the value of the taxonID of the scientificName in the checklist that represents the name originally established under the rules of the associated nomenclatural code, e.g. the basionym)<br><small>**Terms**: [dwc:originalNameUsageID](https://dwc.tdwg.org/list/#dwc_originalNameUsageID)</small><br>


**Rank unknown** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=RANK_INVALID&advanced=1)</small><br>The value for taxonRank could not be interpreted. Check if you can map the value to one of [the accepted taxon rank values](https://api.gbif.org/v1/enumeration/basic/Rank)<br><small>**Terms**: [dwc:taxonRank](https://dwc.tdwg.org/list/#dwc_taxonRank)</small><br>


**Nomenclatural status unknown** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=NOMENCLATURAL_STATUS_INVALID&advanced=1)</small><br>The value for nomenclaturalStatus could not be interpreted. Check if you can map the value to one of [the accepted nomenclatural status values](https://api.gbif.org/v1/enumeration/basic/NomenclaturalStatus)<br><small>**Terms**: [dwc:nomenclaturalStatus](https://dwc.tdwg.org/list/#dwc_nomenclaturalStatus)</small><br>


**Taxonomic status unknown** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=TAXONOMIC_STATUS_INVALID&advanced=1)</small><br>The value for taxonomicStatus could not be interpreted. Check if you can map the value to one of [the accepted taxonomic status values](https://api.gbif.org/v1/enumeration/basic/TaxonomicStatus)<br><small>**Terms**: [dwc:TaxonomicStatus](https://dwc.tdwg.org/list/#dwc_taxonomicStatus)</small><br>


**ScientificName assembled** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=SCIENTIFIC_NAME_ASSEMBLED&advanced=1)</small><br>The scientific name was assembled from the individual name components (e.g. genus name, species epithet, authors), and not supplied as a whole. This is simply for information, publishers can ignore it.<br><small>**Terms**: [dwc:scientificName](https://dwc.tdwg.org/list/#dwc_scientificName)</small><br>


**Chained synonym** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=CHAINED_SYNOYM&advanced=1)</small><br>The record is a synonym which has another synonym as it's accepted name. The GBIF system resolves such chains and links every synonym to the final accepted name. Check that synonyms always point to accepted names.<br><small>**Terms**: [dwc:acceptedNameUsageID](https://dwc.tdwg.org/list/#dwc_acceptedNameUsageID)</small><br>


**Basionym author mismatch** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=BASIONYM_AUTHOR_MISMATCH&advanced=1)</small><br>The authorship of the original name does not match the authorship in brackets of the taxon name. This flags the relationship between the two names as suspicious, based on the formal rules defined by nomenclatural codes.<br><small>**Terms**: [dwc:scientificName](https://dwc.tdwg.org/list/#dwc_scientificName), [dwc:scientificNameAuthorship](https://dwc.tdwg.org/list/#dwc_scientificNameAuthorship), [dwc:originalNameUsage](https://dwc.tdwg.org/list/#dwc_originalNameUsage)</small><br>


<!--- 
**Taxonomic status mismatch**
This flag is not implemented yet! 
 <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=TAXONOMIC_STATUS_MISMATCH&advanced=1)</small><br>The taxonomic status of a name is based on taxonomic opinion. In combination of data from various sources, taxonomic opinions can differ. This flag alerts to seeming inconsistencies within a group of names.<br><small>**Terms**: [dwc:taxonomicStatus](https://dwc.tdwg.org/list/#dwc_taxonomicStatus)</small><br>
--->

**Classification parent cycle** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=PARENT_CYCLE&advanced=1)</small><br>The child-parent relationships between taxon names result in a cycle that needs to be resolved/cut. The classification should be a tree.<br><small>**Terms**: [dwc:parentNameUsageID](https://dwc.tdwg.org/list/#dwc_parentNameUsageID)</small><br>


<!--- 
**Classification rank order invalid**
This flag is not implemented yet! 
 <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=CLASSIFICATION_RANK_ORDER_INVALID&advanced=1)</small><br>The taxon names in a child-parent chain are out of sequence relating to their ranks. Make sure that each child taxon points to its direct parent, as represented in the checklist.<br><small>**Terms**: [dwc:parentNameUsageID](https://dwc.tdwg.org/list/#dwc_parentNameUsageID), [dwc:taxonRank](https://dwc.tdwg.org/list/#dwc_taxonRank)</small><br>
--->

**Classification not applied** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=CLASSIFICATION_NOT_APPLIED&advanced=1)</small><br>The denormalized classification of the checklist, i.e. values for terms like dwc:family or dwc:phylum, could not be applied to the name safely. This usually happens if there is also a normalised parentNameUsageID-based classification given with unspecified ranks.<br><small>**Terms**: [dwc:parentNameUsageID](https://dwc.tdwg.org/list/#dwc_parentNameUsageID)</small><br>


**Vernacular name invalid** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=VERNACULAR_NAME_INVALID&advanced=1)</small><br>At least one part of a vernacular name attached to this taxon name, linked from the Vernacular Names extension, could not be interpreted. This usually happens when  the name was blank, but it is also flagged if other controlled values such as language, lifestage, plural or sex in a Vernacular Name record cannot be interpreted. <br><small>**Terms**: [dwc:vernacularName](https://dwc.tdwg.org/list/#dwc_vernacularName) and [Vernacular Names](https://tools.gbif.org/dwca-validator/extension.do?id=gbif:VernacularName) extension http://rs.gbif.org/extension/gbif/1.0/vernacularname.xml</small><br>


**Description invalid** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=DESCRIPTION_INVALID&advanced=1)</small><br>At least one description record for this taxon name, linked from the Taxon Description extension, could not be interpreted because the mandatory description was missing or the language field was invalid.<br><small>**Terms**: [Taxon Description](https://tools.gbif.org/dwca-validator/extension.do?id=gbif:Description) extension http://rs.gbif.org/extension/gbif/1.0/description.xml</small><br>


**Distribution invalid** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=DISTRIBUTION_INVALID&advanced=1)</small><br>At least one species distribution record for this taxon name, linked from the Species Distribution extension, could not be interpreted.<br><small>**Terms**: [Species Distribution](https://tools.gbif.org/dwca-validator/extension.do?id=gbif:Distribution) extension https://rs.gbif.org/extension/gbif/1.0/distribution.xml</small><br>


**Species profile invalid** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=SPECIES_PROFILE_INVALID&advanced=1)</small><br>At least one species profile record for this taxon name, linked from the Species Profile extension, could not be interpreted.<br><small>**Terms**: [Species Profile](https://tools.gbif.org/dwca-validator/extension.do?id=gbif:SpeciesProfile) extension https://rs.gbif.org/extension/gbif/1.0/speciesprofile_2019-01-29.xml</small><br>


**Multimedia invalid** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=MULTIMEDIA_INVALID&advanced=1)</small><br>At least one multimedia extension record attached to this taxon name could not be interpreted. This covers multimedia coming in through various [extensions](https://tools.gbif.org/dwca-validator/extensions.do), including Audubon core, Simple images or multimedia or EOL media.<br><small>**Terms**: See https://data-blog.gbif.org/post/gbif-multimedia/</small><br>


**Bibliographic references invalid** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=BIB_REFERENCE_INVALID&advanced=1)</small><br>At least one bibliographic reference for this taxon name, linked from the Literature References extension, could not be interpreted.<br><small>**Terms**: [Literature References](https://tools.gbif.org/dwca-validator/extension.do?id=gbif:Reference) Extension https://rs.gbif.org/extension/gbif/1.0/references.xml</small><br>


**Alternative identifiers invalid** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=ALT_IDENTIFIER_INVALID&advanced=1)</small><br>At least one alternative identifier for this taxon name, linked from the Alternative Identifiers extension, could not be interpreted.<br><small>**Terms**: [Alternative Identifiers](https://tools.gbif.org/dwca-validator/extension.do?id=gbif:Identifier) extension https://rs.gbif.org/extension/gbif/1.0/identifier.xml</small><br>


**Could not be matched to GBIF backbone** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=BACKBONE_MATCH_NONE&advanced=1)</small><br>The interpretation of the taxonomic name could not find an existing equivalent, or near-enough match, in the GBIF taxonomic backbone. If the taxon name is newly described or a recent recombination, this is expected, until the new name can be integrated into the backbone taxonomy. You can check how a scientific name is matched against the backbone taxonomy using our species name matching tool: https://www.gbif.org/tools/species-lookup<br><small>**Terms**: [dwc:scientificName](https://dwc.tdwg.org/list/#dwc_scientificName)</small><br>


**Fuzzy GBIF backbone match** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=BACKBONE_MATCH_FUZZY&advanced=1)</small><br>Name match to the GBIF backbone taxonomy could only be done using a fuzzy, non exact match.<br><small>**Terms**: [dwc:scientificName](https://dwc.tdwg.org/list/#dwc_scientificName)</small><br>


**Synonym lacking an accepted name** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=ACCEPTED_NAME_MISSING&advanced=1)</small><br>The taxon name is explicitly marked as a synonym, but lacking a reference to the corresponding accepted name. If the accepted name is contained in the same data source, consider adding a reference to it.<br><small>**Terms**: [dwc:TaxonomicStatus](https://dwc.tdwg.org/list/#dwc_taxonomicStatus), [dwc:acceptedNameUsageID](https://dwc.tdwg.org/list/#dwc_acceptedNameUsageID)</small><br>


**Accepted name not unique** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=ACCEPTED_NAME_NOT_UNIQUE&advanced=1)</small><br>The synonym record provides the accepted name as verbatim text, rather than as a cross-reference. The verbatim name is ambiguous and could refer to several different records in GBIF's backbone taxonomy.<br><small>**Terms**: [dwc:acceptedNameUsage](https://dwc.tdwg.org/list/#dwc_acceptedNameUsage)</small><br>


**Parent name not unique** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=PARENT_NAME_NOT_UNIQUE&advanced=1)</small><br>The record provides the name of the taxonomic parent as verbatim text, rather than as a cross-reference. The verbatim name is ambiguous and could refer to several different records in GBIF's backbone taxonomy.<br><small>**Terms**: [dwc:parentNameUsage](https://dwc.tdwg.org/list/#dwc_parentNameUsage)</small><br>


**Original name not unique** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=ORIGINAL_NAME_NOT_UNIQUE&advanced=1)</small><br>The record provides the original name of the taxon (e.g. basionym) as verbatim text, rather than as a cross-reference. The verbatim name is ambiguous and could refer to several different records in GBIF's backbone taxonomy.<br><small>**Terms**: [dwc:originalNameUsage](https://dwc.tdwg.org/list/#dwc_originalNameUsage)</small><br>


**Relationship missing** <small>(checklist)</small> <small>[example](https://www.gbif.org/species/search?issue=RELATIONSHIP_MISSING&advanced=1)</small><br>There were problems representing all name relationships, i.e. the link to the parent, accepted and/or original name. The interpreted record in GBIF is lacking some of the original source relation.<br><small>**Terms**: [dwc:originalNameUsage](https://dwc.tdwg.org/list/#dwc_originalNameUsage), [dwc:parentNameUsage](https://dwc.tdwg.org/list/#dwc_parentNameUsage), [dwc:acceptedNameUsage](https://dwc.tdwg.org/list/#dwc_acceptedNameUsage), [dwc:acceptedNameUsageID](https://dwc.tdwg.org/list/#dwc_acceptedNameUsageID), [dwc:TaxonomicStatus](https://dwc.tdwg.org/list/#dwc_taxonomicStatus), [dwc:parentNameUsageID](https://dwc.tdwg.org/list/#dwc_parentNameUsageID)</small><br>


**Basionym relation derived** <small>(GBIF backbone)</small> <small>[example](https://www.gbif.org/species/search?issue=ORIGINAL_NAME_DERIVED&advanced=1)</small><br>The record in GBIF has a relationship to an original name (basionym) that was derived from name & authorship comparison, but did not exist explicitly in the source data. This will only be flagged in programmatically generated GBIF backbone records of name usages. GBIF backbone specific issue.


**Conflicting basionym combination** <small>(GBIF backbone)</small> <small>[example](https://www.gbif.org/species/search?issue=CONFLICTING_BASIONYM_COMBINATION&advanced=1)</small><br>There have been more than one accepted name in a homotypical basionym group of names. GBIF backbone specific issue.<br><small>**Terms**: [dwc:scientificName](https://dwc.tdwg.org/list/#dwc_scientificName)</small><br>


**No species included** <small>(GBIF backbone)</small> <small>[example](https://www.gbif.org/species/search?issue=NO_SPECIES&advanced=1)</small><br>The group (currently only genera are tested) is lacking any accepted species. GBIF backbone specific issue.


**Name parent mismatch** <small>(GBIF backbone)</small> <small>[example](https://www.gbif.org/species/search?issue=NAME_PARENT_MISMATCH&advanced=1)</small><br>The (accepted) bi/trinomial name does not match the parent name and should be recombined into the parent genus/species. For example the species _Picea alba_ with a parent genus _Abies_ is a mismatch, and should be replaced by _Abies alba_. GBIF backbone specific issue.


**Orthographic variant** <small>(GBIF backbone)</small> <small>[example](https://www.gbif.org/species/search?issue=ORTHOGRAPHIC_VARIANT&advanced=1)</small><br>An entry in the backbone is suspected to be only a spelling variation of an otherwise existing name. GBIF backbone specific issue.


**Homonym** <small>(GBIF backbone)</small> <small>[example](https://www.gbif.org/species/search?issue=HOMONYM&advanced=1)</small><br>A not synonymized homonym exists for this name in some other backbone source which have been ignored at build time. GBIF backbone specific issue.
<!--- 
TODO: Need a better explanation. Why not-synonymized, and ignored? What does it mean to a user? I would have assumed this flag to notify users that a homonym (identical name but different authorship) does exist that describes an unrelated group of organisms (taxon).
--->

**Published earlier than parent name** <small>(GBIF backbone)</small> <small>[example](https://www.gbif.org/species/search?issue=PUBLISHED_BEFORE_GENUS&advanced=1)</small><br>A bi/trinomial name was seemingly published earlier than the parent genus/species. This might indicate a homonym issue, or that the name should rather be a recombination. GBIF backbone specific issue.
