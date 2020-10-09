---
title: GBIF Issues & Flags
author: Leonardo Buitrago
date: '2020-10-08'
slug: issues-and-flags
categories:
  - GBIF
  - Data Quality
tags:

  - GBIF
  - issues
  - flags
  - data quality

lastmod: '2020-10-08'
draft: yes
keywords: ['issues', 'flags', 'quality', 'interpretation', 'users', 'publishers', 'GBIF']
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

> "With great power comes great responsibility", is a proverb that emerged during French Revolution but popularized by the Spider-Man comic books written by Stan Lee. 


This slogan can be applied also when we share and use biodiversity data, especially through a worldwide platform as GBIF.org. Publishers play an essential role not simply in sharing datasets, but also in managing their quality, completeness and usefulness and ensuring their integration and value within GBIF’s global knowledge base (GBIF.org, 2020). However, as Chapman highlight, sometimes data are used without consideration of the error contained within, and this can lead to erroneous results, misleading information, unwise environmental decisions and increased costs. So, users have a big responsibility too when using the published data. The publishing, access and use of biodiversity data involve mainly these three actors in a data workflow and each one plays an important role to prevent and improve the data quality.

![](/post/2020-10-09-issues-and-flags_files/workflow1.png)

## What are GBIF issues and flags?
GBIF harvests datasets, integrating them into a common access system where users can retrieve any and all data through common search and download services. During this indexation process, GBIF.org performs, over the raw data, additional checks and conversion routines (we call it “interpretation”) to ensure that data are interoperable and comply with minimum standards of data formats, data quality and fitness for use.

![](/post/2020-10-09-issues-and-flags_files/workflow2.png)

All these routines become an effective mechanism to detect and filter, as “issues and flags”, some of the regular or most common data quality problems, mainly addressed to meet the following goals (depending on the role):

> + For users: to filter and be aware of possible inconsistencies to use the data.
> + For data providers: help to detect different quality issues that can be fixed in the published dataset/records, and therefore, possible actions to take to improve the data quality.
> + For GBIF: to recognize the interpretation quality process the portal is doing over the indexed records.

![](/post/2020-10-09-issues-and-flags_files/issues&flags_main.png)

Thinking on how publishers and users can deal with the issues/flags identified by GBIF.org is important to recognize the most common problems that this mechanism can help to improve the data quality:

- Lack in the use of controlled vocabularies (e.g. basisOfRecord)
- Inconsistent data (e.g. country ≠ countryCode)
- Using appropriate standardized formats, like ISO codes (e.g. countryCode, eventDate)
- Using valid values to interpret/index numeric data (e.g. individualCount, coordinateUncertaintyInMeters)
- Values between the correct numeric range laid in the right columns (e.g. elevation/depth swapped)
- Inappropriate use of zero (0) to document empty or missing values (e.g. decimal Lat/Long = 0)
- Using correct characters to build lists (e.g. references, url)

### For users
Any user can recognize very easily the issues and flags in GBIF.org:

_(at home page)_

1. **Filtering records**, using the “issues and flags” in the search occurrences page is possible selecting the records with the issue or excluding them using the reverse function, or simply clear the selection.

![](/post/2020-10-09-issues-and-flags_files/filtering.png)

_(at occurrence page)_

2. Some coloured pills appear in the “Remarks” column, showing the interpretation result in a record field. The _"issues"_ that are the problems strongly recommended to be aware of the data-use (or to be fixed by the publishers) appear in yellow color; and _"flags"_ that highlight the simple adjustments that GBIF does after interpretation are showing in grey color.
Three different remarks are showing to explain the process done after interpretation:
- **Excluded**: The original data couldn’t be interpreted by GBIF.org so it’s excluded in the interpreted fields.

![](/post/2020-10-09-issues-and-flags_files/excluded.png)

- **Altered**: The original data is modified in the interpretation process to be indexed in GBIF.org.

![](/post/2020-10-09-issues-and-flags_files/altered.png)

- **Inferred**: Using other record information the data indexed is inferred, if the original is empty.

![](/post/2020-10-09-issues-and-flags_files/inferred.png)

### For Data providers (“publishers”)

A list of issues & flags is also available for each dataset at the dataset page metrics (e.g. https://www.gbif.org/dataset/4fa7b334-ce0d-4e88-aaae-2e0c138d049e/metrics). This help to publishers to identify what are exactly the data quality issues to tackle.

![](/post/2020-10-09-issues-and-flags_files/dataset.png)

## How to improve data quality using issues and flags?
Although all listed issues are important to publish and use better data, there are some of them that could be a priority for GBIF data providers and offer greater added value if they can be tackled properly; taking into account also the total number of records in GBIF.org currently affected. The next table help publishers to know how to fix these main issues to improve the data quality:

<style>
#issues {
  font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
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
<td align="left">Use the columns <strong>countryCode</strong> and <strong>country</strong> with the country information where the record was registered, following the officially ISO 3166-1-alpha-2 country code</td>
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
<td align="left">Leave <strong>decimalLatitude</strong> and <strong>decimalLongitude</strong> in blank if the coordinates are missing. Don't use "0" as a coordinate value in both columns unless your record be present there (Null Island <a href="https://en.wikipedia.org/wiki/Null_Island">https://en.wikipedia.org/wiki/Null_Island</a>).</td>
</tr>
<tr>
<td align="left">Coordinate invalid</td>
<td align="left">Make sure coordinates are valid numeric decimal values. <strong>decimalLatitude</strong>: legal values lie between -90 and 90, inclusive. and <strong>decimalLongitude</strong>: legal values lie between -180 and 180, inclusive. Also, <strong>verbatimCoordinates</strong> have to be valid values for coordinates in decimal degrees, degrees decimal minutes, degrees minutes second</td>
</tr>
</tbody>
</table>


## Which are all issues and flags developed by GBIF.org?
More than 50 issues and flags have been created to deal with the occurrences data quality common problems. The next table compiles all of them and offer a more clear description of each one and let to know what terms are involved with the issue/flag and shows a link to the GBIF records affected that help as an explicit example.

<table id = "issues">
<thead>
<tr>
<th><strong>Issue &amp; flag</strong></th>
<th><strong>Description</strong></th>
<th><strong>Relates to</strong></th>
<th><strong>Examples</strong></th>
<th><strong>Category</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Coordinate rounded</td>
<td>In the data interpretation the original coordinates are round to 5 decimals. This is equivalent to 1.11m; any further digits would give a false sense of precision, considering the error range of measuring devices.</td>
<td>dwc:decimalLatitude, dwc:decimalLongitude</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COORDINATE_ROUNDED">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Geodetic datum assumed WGS84</td>
<td>If the datum is null, data interpretation assumes the record coordinates relate to WGS84.</td>
<td>dwc:geodeticDatum</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=GEODETIC_DATUM_ASSUMED_WGS84">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Country derived from coordinates</td>
<td>If the country and country code are not supplied or cannot be matched to known values, data interpretation derives their content from the decimal coordinates through a lookup service.</td>
<td>dwc:countryCode, dwc:country, dwc:decimalLatitude, dwc:decimalLongitude</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COUNTRY_DERIVED_FROM_COORDINATES">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Continent derived from coordinates</td>
<td>If no value is supplied for the continent or if the values cannot be matched against a known vocabulary, data interpretation derives the continent from the decimal coordinates.</td>
<td>dwc:continent, dwc:decimalLatitude, dwc:decimal Longitude</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=CONTINENT_DERIVED_FROM_COORDINATES">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Taxon match higherrank</td>
<td>The record can be matched to the GBIF taxonomic backbone at a higher rank, but not with the scientific name given. Some possible reasons include:<br/>- The name is new, and not available in the taxonomic datasets yet<br/>- The name is missing in the backbone's taxonomic sources for others reasons (outdated information)<br/>- Formatting or spelling of the scientific name caused interpretation errors</td>
<td>dwc:scientificName,dwc:kingdom,dwc:phylum, dwc:class, dwc:order, dwc:family, dwc:genus, dwc:subgenus, dwc:specificEpithet, dwc:infraspecificEpithet, dwc:taxonRank</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=TAXON_MATCH_HIGHERRANK">example</a></td>
<td>TAXONOMIC</td>
</tr>
<tr>
<td>Country invalid</td>
<td>The country given cannot be matched to the vocabulary for country names</td>
<td>dwc:country</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COUNTRY_INVALID">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Continent invalid</td>
<td>The continent given cannot be matched to the vocabulary for continent names</td>
<td>dwc:continent</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=CONTINENT_INVALID">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>References URI invalid</td>
<td>The references URL cannot be resolved, and may be malformed or contain invalid characters. If there is more than one URL, the values have to be comma-separated.</td>
<td>dc:references</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=REFERENCES_URI_INVALID">example</a></td>
<td>URI</td>
</tr>
<tr>
<td>Recorded date invalid</td>
<td>The recording date given cannot be intrepreted because is invalid. Most common reasons include:<br/>- A non-existing date (e.g "1995-04-34")<br/>- Missing date parts (e.g. Event date without year).<br/>- The date format does not follow the ISO 8601 standard (YYYY-MM-DD)</td>
<td>dwc:eventDate, dwc:year, dwc:month, dwc:day</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=RECORDED_DATE_INVALID">example</a></td>
<td>DATE</td>
</tr>
<tr>
<td>Geodetic datum invalid</td>
<td>The geodetic datum could not be interpreted, because the supplied term cannot be matched against the vocabulary of known values.</td>
<td>dwc:geodeticDatum</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=GEODETIC_DATUM_INVALID">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Coordinate precision invalid</td>
<td>Indicates an invalid or very unlikely coordinates precision. The value is not a decimal number as expected, or it is unusually low or high for a margin of uncertainty.</td>
<td>dwc:coordinatePrecision</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COORDINATE_PRECISION_INVALID">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Basis of record invalid</td>
<td>The given basis of record is impossible to interpret or seriously different from the recommended vocabulary: <a href="http://rs.gbif.org/vocabulary/dwc/basis_of_record.xml">http://rs.gbif.org/vocabulary/dwc/basis_of_record.xml</a></td>
<td>dwc:basisOfRecord</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=BASIS_OF_RECORD_INVALID">example</a></td>
<td>VOCABULARY</td>
</tr>
<tr>
<td>Recorded date mismatch</td>
<td>The recording date specified as the eventDate string and the individual year, month, day are contradicting.</td>
<td>dwc:eventDate, dwc:year, dwc:month, dwc:day</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=RECORDED_DATE_MISMATCH">example</a></td>
<td>DATE</td>
</tr>
<tr>
<td>Taxon match none</td>
<td>Matching to the taxonomic backbone cannot be done cause there was no match at all or several matches with too little information to keep them apart (homonyms).</td>
<td>dwc:scientificName,dwc:kingdom,dwc:phylum, dwc:class, dwc:order, dwc:family, dwc:genus, dwc:subgenus, dwc:specificEpithet, dwc:infraspecificEpithet, dwc:taxonRank</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=TAXON_MATCH_NONE">example</a></td>
<td>TAXONOMIC</td>
</tr>
<tr>
<td>Taxon match fuzzy</td>
<td>Matching to the taxonomic backbone can only be done using a fuzzy, non exact match.</td>
<td>dwc:scientificName,dwc:kingdom,dwc:phylum, dwc:class, dwc:order, dwc:family, dwc:genus, dwc:subgenus, dwc:specificEpithet, dwc:infraspecificEpithet, dwc:taxonRank</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=TAXON_MATCH_FUZZY">example</a></td>
<td>TAXONOMIC</td>
</tr>
<tr>
<td>Type status invalid</td>
<td>The given type status is impossible to interpret or seriously different from the recommended vocabulary: <a href="https://rs.gbif.org/vocabulary/gbif/type_status.xml">https://rs.gbif.org/vocabulary/gbif/type_status.xml</a></td>
<td>dwc:typeStatus</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=TYPE_STATUS_INVALID">example</a></td>
<td>VOCABULARY</td>
</tr>
<tr>
<td>Coordinate reprojected</td>
<td>The original coordinates were successfully reprojected from a different geodetic datum to WGS84.</td>
<td>dwc:geodeticDatum</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COORDINATE_REPROJECTED">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Country coordinate mismatch</td>
<td>The interpreted occurrence coordinates fall outside of the indicated country.</td>
<td>dwc:countryCode, dwc:country, dwc:decimalLatitude, dwc:decimalLongitude</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COUNTRY_COORDINATE_MISMATCH">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Individual count invalid</td>
<td>Individual count value not parsable into an integer.</td>
<td>dwc:individualCount</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=INDIVIDUAL_COUNT_INVALID">example</a></td>
<td>INDIVIDUAL_COUNT</td>
</tr>
<tr>
<td>Elevation min max swapped</td>
<td>The values for minimum and maximum elevation appear to the swapped.</td>
<td>dwc:minimumElevationInMeters, dwc:maximumElevationInMeters</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=ELEVATION_MIN_MAX_SWAPPED">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Coordinate uncertainty meters invalid</td>
<td>The value given for Coordinate uncertainty in meters, indicating the radius of uncertainty around the given decimal coordinates, is not a valid number, or lies outside a plausible range.</td>
<td>dwc:coordinateUncertaintyInMeters</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COORDINATE_UNCERTAINTY_METERS_INVALID">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Modified date unlikely</td>
<td>The modified date given is in the future or predates unix time (1970).</td>
<td>dc:modified</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=MODIFIED_DATE_UNLIKELY">example</a></td>
<td>DATE</td>
</tr>
<tr>
<td>Zero coordinate</td>
<td>Coordinate is the exact 0/0 coordinate, often indicating a bad null coordinate.</td>
<td>dwc:decimalLatitude, dwc:decimalLongitude</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=ZERO_COORDINATE">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Coordinate invalid</td>
<td>A coordinate value is given in some form, but GBIF is unable to interpret it. Possible reasons include, i.a., coordinates that fall out of range (larger/lower than 90/-90 or 180/-180, depending) or text values that cannot be interpreted.</td>
<td>dwc:decimalLatitude, dwc:decimalLongitude, dwc:verbatimCoordinates, dwc:verbatimLatitude, dwc:verbatimLongitude</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COORDINATE_INVALID">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Country mismatch</td>
<td>Interpreted Country and Country code contradict each other.</td>
<td>dwc:countryCode, dwc:country</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COUNTRY_MISMATCH">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Identified date unlikely</td>
<td>The identification date is in the future or before Linnean times (1700).</td>
<td>dwc:dateIdentified</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=IDENTIFIED_DATE_UNLIKELY">example</a></td>
<td>DATE</td>
</tr>
<tr>
<td>Recorded Date Unlikely</td>
<td>The recording date is highly unlikely, falling either into the future or representing a very old date before 1600 that predates modern taxonomy.</td>
<td>dwc:eventDate, dwc:year, dwc:month, dwc:day</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=RECORDED_DATE_UNLIKELY">example</a></td>
<td>DATE</td>
</tr>
<tr>
<td>Depth min max swapped</td>
<td>The values for minimum and maximum depth appear to the swapped.</td>
<td>dwc:minimumDepthInMeters, dwc:maximumDepthInMeters</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=DEPTH_MIN_MAX_SWAPPED">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Presumed negated longitude</td>
<td>The supplied longitude value places the coordinates outside of the indicated country. Negating the longitude value would result in a country match.</td>
<td>dwc:decimalLongitude</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=PRESUMED_NEGATED_LONGITUDE">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Presumed swapped coordinate</td>
<td>Coordinates seem to be swapped when testing against the interpreted country.</td>
<td>dwc:decimalLatitude, dwc:decimalLongitude, dwc:country</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=PRESUMED_SWAPPED_COORDINATE">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Multimedia URI invalid</td>
<td>The multimedia URL cannot be resolved, and may be malformed or contain invalid characters. If there is more than one URL, the values have to be comma-separated.</td>
<td>dwc:associatedMedia</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=MULTIMEDIA_URI_INVALID">example</a></td>
<td>URI</td>
</tr>
<tr>
<td>Depth non numeric</td>
<td>The values for minimum and maximum depth are non-numeric values and cannot be interpreted.</td>
<td>dwc:minimumDepthInMeters, dwc:maximumDepthInMeters</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=DEPTH_NON_NUMERIC">https://www.gbif.org/occurrence/search?issue=DEPTH_NON_NUMERIC</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Depth unlikely</td>
<td>The values for minimum and maximum depth are negative or higher than 11000 (Mariana Trench depth in meters)</td>
<td>dwc:minimumDepthInMeters, dwc:maximumDepthInMeters</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=DEPTH_UNLIKELY">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Presumed negated latitude</td>
<td>The supplied latitude value places the coordinates outside of the indicated country. Negating the latitude value would result in a country match.</td>
<td>dwc:decimalLatitude</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=PRESUMED_NEGATED_LATITUDE">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Coordinate out of range</td>
<td>The supplied coordinates lie outside of the range for decimal lat/lon values (-90/90, -180/180).</td>
<td>dwc:decimalLatitude, dwc:decimalLongitude, dwc:verbatimCoordinates, dwc:verbatimLatitude, dwc:verbatimLongitude</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COORDINATE_OUT_OF_RANGE">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Elevation non numeric</td>
<td>The values for minimum and maximum elevation are non-numeric values and cannot be interpreted.</td>
<td>dwc:minimumElevationInMeters, dwc:maximumElevationMeters</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=ELEVATION_NON_NUMERIC">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Coordinate reprojection suspicious</td>
<td>Indicates successful coordinate reprojection according to provided datum, but which results in a datum shift larger than 0.1 decimal degrees.</td>
<td>dwc:geodeticDatum, dwc:decimalLatitude, dwc:decimalLongitude</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COORDINATE_REPROJECTION_SUSPICIOUS">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Depth not metric</td>
<td>Set if supplied depth is not given in the metric system, for example using feet instead of meters</td>
<td>dwc:minimumDepthInMeters, dwc:maximumDepthInMeters</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=DEPTH_NOT_METRIC">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Elevation not metric</td>
<td>Set if supplied elevation is not given in the metric system, for example using feet instead of meters</td>
<td>dwc:minimumElevationInMeters, dwc:maximumElevationInMeters</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=ELEVATION_NOT_METRIC">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Multimedia date invalid</td>
<td>The creation date given cannot be intrepreted because is invalid. Most common reasons include:<br/>- A non-existing date (e.g "1995-04-34")<br/>- Missing date parts (e.g. Event date without year).<br/>- The date format does not follow the ISO 8601 standard (YYYY-MM-DD)</td>
<td>dc:created</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=MULTIMEDIA_DATE_INVALID">example</a></td>
<td>DATE</td>
</tr>
<tr>
<td>Coordinate reprojection failed</td>
<td>The given decimal latitude and longitude could not be reprojected to WGS84 based on the provided datum.</td>
<td>dwc:geodeticDatum, dwc:decimalLatitude, dwc:decimalLongitude</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COORDINATE_REPROJECTION_FAILED">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Coordinate accuracy invalid</td>
<td>Deprecated.</td>
<td></td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COORDINATE_ACCURACY_INVALID">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Coordinate precision uncertainty mismatch</td>
<td>Deprecated.</td>
<td></td>
<td><a href="https://www.gbif.org/occurrence/search?issue=COORDINATE_PRECISION_UNCERTAINTY_MISMATCH">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Identified date invalid</td>
<td>The identification date given cannot be intrepreted because is invalid. Most common reasons include:<br/>- A non-existing date (e.g "1995-04-34")<br/>- Missing date parts (e.g. without year).<br/>- The date format does not follow the ISO 8601 standard (YYYY-MM-DD)</td>
<td>dwc:dateIdentified</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=IDENTIFIED_DATE_INVALID">example</a></td>
<td>DATE</td>
</tr>
<tr>
<td>Interpretation error</td>
<td>An error occurred during interpretation, leaving the record interpretation incomplete.</td>
<td>GBIF interpretation</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=INTERPRETATION_ERROR">example</a></td>
<td>INTERPRETATION</td>
</tr>
<tr>
<td>Continent country mismatch</td>
<td>The interpreted continent and country do not match up.</td>
<td>dwc:continent, dwc:countryCode, dwc:country</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=CONTINENT_COUNTRY_MISMATCH">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Elevation unlikely</td>
<td>The values for minimum and maximum elevation are above the troposphere (17000 m) or below Mariana Trench (11000 m)</td>
<td>dwc:minimumElevationInMeters, dwc:maximumElevationInMeters</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=ELEVATION_UNLIKELY">example</a></td>
<td>GEOSPATIAL</td>
</tr>
<tr>
<td>Modified date invalid</td>
<td>A (partial) invalid modified date is given. Most common reasons include:<br/>- A non-existing date (e.g ""1995-04-34"")<br/>- Missing date parts (e.g. without year).<br/>- The date format does not follow the ISO 8601 standard (YYYY-MM-DD)"</td>
<td>dc:modified</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=MODIFIED_DATE_INVALID">example</a></td>
<td>DATE</td>
</tr>
<tr>
<td>Georeferenced date invalid</td>
<td>The georeference date given cannot be intrepreted because is invalid. Most common reasons include:<br/>- A non-existing date (e.g "1995-04-34")<br/>- Missing date parts (e.g. without year).<br/>- The date format does not follow the ISO 8601 standard (YYYY-MM-DD)</td>
<td>dwc:georeferencedDate</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=GEOREFERENCED_DATE_INVALID">example</a></td>
<td>DATE</td>
</tr>
<tr>
<td>Georeferenced date unlikely</td>
<td>The georeference date given is in the future or before Linnean times (1700).</td>
<td>dwc:georeferencedDate</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=GEOREFERENCED_DATE_UNLIKELY">example</a></td>
<td>DATE</td>
</tr>
<tr>
<td>Individual count conflicts with occurrence status</td>
<td>The values given for the individual count and for the status of the occurrence (present/absent) contradict each other (e.g. the count is 0 but the status says "present")</td>
<td>dwc:individualCount, dwc:occurrenceStatus</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=INDIVIDUAL_COUNT_CONFLICTS_WITH_OCCURRENCE_STATUS">example</a></td>
<td>INDIVIDUAL_COUNT</td>
</tr>
<tr>
<td>Occurrence status inferred from individual count</td>
<td>The present/absent status of the occurrence was inferred from the individual count value because no status value was supplied explicitly. An individual count of 0 is interpreted as status="absent", a value > 0 as "present"</td>
<td>dwc:individualCount, dwc:occurrenceStatus</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=OCCURRENCE_STATUS_INFERRED_FROM_INDIVIDUAL_COUNT">example</a></td>
<td>OCCURRENCE_STATUS</td>
</tr>
<tr>
<td>Occurrence status unparsable</td>
<td>The given occurenceStatus value cannot be interpreted; it does not match any of the known (vocabulary) values that indicate the presence or absence of a species at collection or observation event.</td>
<td>dwc:occurrenceStatus</td>
<td><a href="https://www.gbif.org/occurrence/search?issue=OCCURRENCE_STATUS_UNPARSABLE">example</a></td>
<td>VOCABULARY</td>
</tr>
</tbody>
</table>
