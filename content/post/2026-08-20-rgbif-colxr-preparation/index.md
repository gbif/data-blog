---
title: Preparing for rgbif v3.9.0 Moving to COL Extended Release 
author: 
date: '2026-08-20'
slug: rgbif-colxr-preparation
categories: []
tags:
  - rstats
  - gbif
lastmod: '2026-08-20'
draft: yes
keywords: []
description: ''
comment: no
toc: no
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

The newest version of **rgbif** (3.9.0 - planned release to CRAN the end of September) introduces **breaking changes** because the default taxonomy has changed from the **GBIF Backbone Taxonomy** to the **COL (Catalogue of Life) Extended Release**. 

To test out the latest version before it is release to CRAN, you can download the development version:

```R 
# Using remotes
remotes::install_github("ropensci/rgbif", ref = "v3.9.0")

# Or using devtools
devtools::install_github("ropensci/rgbif", ref = "v3.9.0")
```

# What Changed in rgbif 3.9.0

Starting in **rgbif 3.9.0**, the default taxonomy has changed from the **GBIF Backbone Taxonomy** to the **COL (Catalogue of Life) Extended Release**. You can read more about this on a previous [data blog post](https://data-blog.gbif.org/post/catalogue-of-life-taxonomic-backbone/). 

This is a **breaking change** that affects how taxonomic keys are returned and how you should structure your queries.

## Key Differences

The **GBIF Backbone Taxonomy** (the old default) uses **numeric** taxon keys such as `5231190` or `2977832`. This taxonomy is now out-of-date compared to COL XR. The GBIF backbone will not be updated. If you still want to continue to use the GBIF Backbone, explicitly set `checklistKey = "d7dddbf4-2cf0-4f39-9b2a-bb099caae36c"`.

The **COL XR** (the new default) uses **alpha-numeric** taxon keys such as `"Q2M4"` or `"9WLSS"`. This is the default behavior with `checklistKey = "7ddf754f-d193-4cc9-b351-99906754a03b"`. 

## Backward Compatibility

**Most of your existing code using numeric keys will probably continue to work.** Functions like `occ_search()` and `occ_download()` automatically detect numeric taxonomic keys and switch to the GBIF Backbone taxonomy with a warning message. 

However, we **strongly recommend** updating your code to use COL XR keys for the most up-to-date taxonomy. Use `gbif_to_col()` to convert your existing GBIF Backbone numeric keys to COL XR alpha-numeric keys.

## Affected Functions

The following functions now use the COL XR by default:

- `name_backbone()` 
- `name_backbone_checklist()` 
- `occ_search()` 
- `occ_download()` 
- `occ_download_prep()` 
- `map_fetch()`
- `mvt_fetch()`

### Name Matching

**Before (rgbif 3.8.5 and earlier):**
```r
library(rgbif)

# Returned GBIF Backbone numeric key
result <- name_backbone(name = "Calopteryx splendens")
result$usageKey
# [1] 5231190  # numeric key
```

**After (rgbif 3.9.0):**
```r
library(rgbif)

# Now returns COL XR alpha-numeric key
result <- name_backbone(name = "Calopteryx splendens")
result$usageKey
# [1] "Q2M4"  # alpha-numeric key
```

### Converting GBIF Backbone Keys to COL XR

If you have existing GBIF Backbone numeric taxon keys and want to convert them to COL XR alpha-numeric keys, use the `gbif_to_col()` convenience function:

```r
# Convert a single GBIF Backbone key
result <- gbif_to_col(5231190)
result$usage$key  # "Q2M4"

# Convert multiple keys at once
results <- gbif_to_col(c(5231190, 2435099, 2877951))
sapply(results, function(x) x$usage$key)
# [1] "Q2M4"  "9WLSS" "Q2N2"
```

This function uses the GBIF species matching API to resolve GBIF Backbone keys to their COL XR equivalents.

### Occurrence Search

**Before (rgbif 3.8.5 and earlier):**
```r
# Using numeric GBIF Backbone key
occ_search(taxonKey = 5231190, limit = 10)
```

**After (rgbif 3.9.0):**
```r
# Use alpha-numeric key
occ_search(taxonKey = "Q2M4", limit = 10)
```

If you provide a numeric taxonomic key (e.g., `taxonKey = 5231190`), rgbif will automatically switch to the GBIF Backbone taxonomy and issue a warning:

```r
occ_search(taxonKey = 5231190, limit = 10)
# Warning: Numeric taxonomic keys detected (taxonKey).
# These are legacy GBIF Backbone identifiers.
# Switching to Backbone checklistKey: d7dddbf4-2cf0-4f39-9b2a-bb099caae36c
# Consider migrating to COL XR identifiers using gbif_to_col().
```

### Occurrence Downloads

**Before (rgbif 3.8.5 and earlier):**
```r
# Using numeric keys with pred()
occ_download(
  pred("taxonKey", 5231190),
  pred("hasCoordinate", TRUE)
)
# With warning about numeric keys
```

**After (rgbif 3.9.0):**
```r
# Using alpha-numeric COL XR keys
occ_download(
  pred("taxonKey", "Q2M4"),
  pred("hasCoordinate", TRUE)
)

# Or get the key dynamically
key <- name_backbone(name = "Calopteryx splendens")$usageKey
occ_download(
  pred("taxonKey", key),
  pred("hasCoordinate", TRUE)
)
```

If you provide numeric taxonomic keys in your download predicates, rgbif will automatically inject the GBIF Backbone checklistKey into each predicate containing numeric keys and issue a warning.

### Batch Name Matching

**Before (rgbif 3.8.5 and earlier):**
```r
species_list <- c("Calopteryx splendens", "Puma concolor", "Quercus robur")

# Returned numeric keys
results <- name_backbone_checklist(species_list)
results$usageKey
# [1] 5231190 2435099 2877951  # numeric keys
```

**After (rgbif 3.9.0):**
```r
species_list <- c("Calopteryx splendens", "Puma concolor", "Quercus robur")

# Returns alpha-numeric COL XR keys
results <- name_backbone_checklist(species_list)
results$usageKey
# [1] "Q2M4" "9WLSS" "Q2N2"  # alpha-numeric keys
```

### Maps

**Before (rgbif 3.8.5 and earlier):**
```r
# Using numeric GBIF Backbone key for penguins
map_fetch(srs='EPSG:3031', taxonKey=2481660, style='glacier.point')
```

**After (rgbif 3.9.0):**
```r
# Get COL XR key first
key <- name_backbone(name = "Spheniscidae")$usageKey
# key is "623RM"

# Use alpha-numeric key
map_fetch(srs='EPSG:3031', taxonKey="623RM", style='glacier.point')
```

## Deprecated Functions

Three functions only work by default with the GBIF Backbone Taxonomy and will show deprecation warnings when a `datasetKey` is not provided.

- `name_lookup()` - Consider using `rcol::col_search()` for COL XR
- `name_suggest()` - Consider using `rcol::col_suggest()` for COL XR
- `name_usage()` - Consider using `rcol::col_usage()` for COL XR

Keep in mind that something like the following will **not** work even if you set the `datasetKey` to COL XR: 

```r 
# will return error
name_usage(key="Q2M4",datasetKey="7ddf754f-d193-4cc9-b351-99906754a03b")

# use instead 
rcol::col_usage("Q2M4")
```

### Feedback or questions

If you have any feedback or questions about the upcoming changes in rgbif 3.9.0, please reach out to the [rgbif GitHub repository](https://github.com/ropensci/rgbif/issues/895). 
