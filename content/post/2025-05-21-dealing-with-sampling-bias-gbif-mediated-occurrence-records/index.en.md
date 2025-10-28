---
title: Dealing with sampling bias GBIF mediated occurrence records
author: John Waller
date: '2025-05-21'
slug: []
categories: []
tags: []
lastmod: '2025-05-21T15:51:13+02:00'
draft: yes
keywords: []
description: ''
authors: ''
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

## Spatial Thinning: Even Out the Spatial Effort

One straightforward way to reduce spatial bias is **spatial thinning** (also called spatial filtering). The idea is to *remove or subsample occurrences that are too close to each other*, so that no locality or region is overrepresented. This addresses the common situation where many records cluster around cities, roads, or popular parks, while other areas have few or none. By thinning the data, we ensure a more even spatial coverage.

A common approach is to impose a minimum distance or grid cell size between records. For example, we might only keep one occurrence **per 5 km radius** or **per grid cell** of a certain size. *“A common approach to reducing spatial bias in occurrence records is to randomly select one (or a small number) of samples present in each cell in the landscape”*. If a keen birder logged 500 observations in one city park, spatial thinning will pick just one (or a few) of those and discard the rest, preventing that park from overwhelming the analysis. This way, areas with intense sampling effort are down-sampled to more closely match areas with low effort.

In R, you can do spatial thinning in various ways. Specialized packages like **`spThin`** or **`GeoThinneR`** implement distance-based thinning algorithms, and the **`dismo`** package offers a simple `gridSample` function. However, you can also use base R or tidyverse tools for a quick solution. For example, to thin occurrences by keeping at most one record per 0.1° grid (≈11 km) you could do:

```r
library(dplyr)

# Example: thin occurrence data to one record per 0.1° grid cell
thinned_data <- occurrences %>%
  mutate(
    lat_bin = round(decimalLatitude, 1),    # bin latitude into 0.1° cells
    lon_bin = round(decimalLongitude, 1)    # bin longitude into 0.1° cells
  ) %>%
  group_by(lat_bin, lon_bin) %>%
  slice_sample(n = 1)    # pick one record per grid cell
```

In the code above, we round coordinates to one decimal place and then randomly select one occurrence for each unique lat-long bin. The result is a thinned dataset with a more uniform spatial distribution of points. You can adjust the grid size (or use a distance in kilometers if converting to a planar coordinate system) based on your needs – larger distances yield stronger thinning.

Spatial thinning is very useful before modeling species distributions or mapping diversity. By removing dense clusters of records, we reduce the chance that our analysis is **skewed towards well-sampled locations**. Essentially, thinning trades off some data (you lose many records from oversampled spots) in exchange for less bias. Even though we end up with fewer points, those points carry more equal weight geographically. This often improves model performance: studies have found that **using spatially thinned data can produce better predictive models** despite using fewer records. In short, thinning helps “level the playing field” across space.

*(Pro tip: If you have hundreds of thousands of points, use packages like `spThin` or `GeoThinneR` which are optimized for efficiency. They offer options for specifying distance thresholds or grid sizes and can handle large datasets.)*

## Effort-Based Filtering: Use (or Impose) Sampling Effort Information

Another tactic is **effort-based filtering**, which means subsetting or weighting the data based on known sampling effort. Not all occurrence records are collected with the same effort: some come from structured surveys where people diligently record *everything* they find in an area, and others are casual or biased observations (e.g. a tourist snapping a photo of a butterfly). By filtering for records with higher or more standardized effort, we can reduce bias from haphazard sampling.

One example comes from bird data. The eBird project (a major contributor of bird records to GBIF) allows birders to submit checklists of sightings. Some of these checklists are **complete** (the observer reports all species they could identify, with details on effort like duration and distance), while others are **incidental** (just reporting a few species opportunistically). It turns out that *“complete checklists with effort information facilitate robust analyses, and thus represent the gold standard of eBird data”*. In practice, researchers often filter eBird-sourced GBIF records to **include only complete checklists** (with at least some minimum effort, say 10 minutes or 1 km of survey). This removes many biased records where effort was low or unreported, yielding a more uniform dataset.

Effort-based filtering can also be as simple as removing obvious duplicate sampling. For instance, if a scientist collected 100 plant specimens from one small site on the same day, those 100 occurrence records don’t represent 100 independent data points – they represent one intensive sampling event. We might choose to keep just one record per site per day in such cases. Here’s how you could filter to one occurrence per location per day in R:

```r
library(dplyr)

# Filter to one record per unique location (lat-long) per day
filtered_data <- occurrences %>%
  mutate(date = as.Date(eventDate)) %>%            # ensure we have a Date column
  arrange(date) %>%
  distinct(decimalLatitude, decimalLongitude, date, .keep_all = TRUE)
```

This code will drop any additional records that share the same lat/long and date, keeping only the first occurrence for each such combination. The rationale is that multiple records from the same place and day likely reflect repeated observations of the same population or a concentrated survey effort. By keeping only one, we down-weight that intense effort to be equivalent to a single observation event.

Effort filtering can be tailored to your data. If you have an **effort metric** (e.g., person-hours, trap nights, transect length), you might exclude records with very low effort or explicitly incorporate effort as a covariate in models. If you know certain datasets are biased (e.g., a museum collection that sampled primarily along roads), you could exclude or down-weight those records. The goal is to make the remaining data more comparable. Essentially, we’re trying to compare apples to apples – for example, one *standardized survey* to another – rather than apples to oranges (casual observations vs. intensive surveys). This reduces bias by ensuring each data point comes from a roughly similar amount of search effort.

## Rarefaction: Standardizing Sample Size for Fair Comparison

**Rarefaction** is a classic ecological method to address sampling bias, especially for comparing species richness across areas or datasets with different sample sizes. The concept is to *subsample* the data to a common sample size and measure diversity from that equal footing. In plainer terms, if Region A has 1,000 occurrence records and Region B has only 100 records, of course A will list more species – but is it because A is truly more diverse, or just more sampled? Rarefaction helps answer that by asking: *if we only had 100 records from Region A, how many species would we expect to find?* This gives a fairer richness comparison between A and B.

One way to do rarefaction is through simulation: randomly draw (say) 100 records from Region A, count how many species are in that subset, and repeat this many times to get an average. A more analytical way uses combinatorics (Hurlbert’s formula) to compute the expected number of species for a given sample size without brute-force simulation. This is often called the **Hurlbert’s index** or ES(n) – expected species for n samples. For example, *ES50* is the expected number of unique species in a random sample of 50 occurrences. **John Waller** (GBIF data scientist) used this metric to compare diversity in his blog, noting that *“es50 ... is an indicator of biodiversity richness”* and importantly *“somewhat corrects for sampling bias”*.

In R, the **`vegan`** package provides functions for rarefaction. We can use `vegan::rarefy()` on a matrix of species counts. For instance, if we have occurrence records split by region or grid cell, we can create a matrix of counts (rows = regions, columns = species). Then we rarefy each region to the same number of samples:

```r
library(vegan)

# Suppose 'occurrences' has a column 'region' and 'species'
# Create a matrix of species counts per region:
region_species_matrix <- table(occurrences$region, occurrences$species)

# Perform rarefaction to a sample size of 100 occurrences per region
rarefied_richness <- rarefy(region_species_matrix, sample = 100)
rarefied_richness
```

The result `rarefied_richness` will be a vector giving the **estimated number of species in each region if we only had 100 records** from that region. Regions with a lot more than 100 records get “down-sampled” in theory, and regions with fewer than 100 records cannot be raised (rarefaction can’t invent species beyond what was observed; typically those will just have their actual species count if they have fewer records than the rarefaction number).

Rarefaction is very powerful for **comparative analyses**. It essentially tells us *what the diversity would be at a common sampling effort*. By doing so, it highlights the places that are truly diverse rather than just well-sampled. For example, applying rarefaction can reveal tropical regions as having higher *underlying* species richness even if their raw record counts are low. This helps counter the bias where, say, temperate zones look richer purely because of more observers. If we rarefy everything to, say, 50 records per area, the playing field is level. In practice, you might use rarefied richness metrics to build maps or to correlate biodiversity with environmental factors without the confounding effect of sampling effort.

One drawback: rarefaction **uses less data** (it ignores records beyond the rarefaction cutoff in well-sampled areas), so you’re intentionally not using all information. Also, if some regions have extremely low sampling (fewer records than the chosen cutoff), you can’t directly rarefy them to the same level – you might have to drop those or choose a smaller uniform sample size. Despite these issues, rarefaction is a widely used technique to mitigate sampling bias in richness studies, and it pairs nicely with the next topic: grid-based mapping.

## Grid-Based Richness Estimation: Apples-to-Apples on the Map

When mapping species richness or other biodiversity metrics, using a spatial **grid** is a common practice to handle sampling bias. By aggregating occurrences into grid cells (e.g. 10×10 km squares or hexagons), you ensure each spatial unit has a more comparable area and sampling effort basis. Grid-based analyses convert raw point data into a form of **presence/absence or counts per cell**, which inherently dampens the influence of duplicate records in the same location.

The simplest grid-based approach is to count the number of species in each cell (species richness per cell). But as noted earlier, if some cells have a ton of records and others have very few, a raw richness map will still be biased – it will light up cells that were sampled to death, and leave poorly-sampled cells looking species-poor (even if in reality they might harbor many unrecorded species). Therefore, it’s common to combine grid-based aggregation **with a threshold or rarefaction**. For example, you might **only map cells that have at least 20 records**, to avoid showing spurious low-richness in cells where you have near-zero effort. Or better yet, apply rarefaction within each cell: calculate something like ES50 for every cell so that each cell’s richness is evaluated at an equal sample size.

&#x20;*An example of grid-based rarefied richness: This map shows the **Hurlbert’s ES50** for animal genera on a global hexagon grid (each hex \~480 km across). Cooler colors (blue/green) mean higher expected genera count in a sample of 50 records, warmer colors (yellow/red) mean lower richness.* Notice how this rarefied richness map highlights the **tropical diversity hotspots** (Central Amazon, Congo, Southeast Asia) even though those areas have fewer total records than temperate zones. By contrast, a raw species count map (not shown here) would mainly highlight places like Europe and the US – essentially mirroring observer distribution. The grid-based ES50 approach helps reveal genuine biodiversity patterns by controlling for sampling effort: *“one of the main advantages of es50 is that it somewhat corrects for sampling bias”*.

To implement grid-based richness in R, you can use packages like **`sf`** or **`raster/terra`** to make spatial grids, or do it with simple grouping operations. For example, using `dplyr` you could assign each occurrence to a grid cell and then count unique species:

```r
library(dplyr)

# Define grid cell IDs, e.g., rounding coordinates to 0.5° for a ~50x50 km grid
occurrences <- occurrences %>%
  mutate(
    lon_bin = floor(decimalLongitude / 0.5) * 0.5,
    lat_bin = floor(decimalLatitude / 0.5) * 0.5,
    cell_id = paste(lat_bin, lon_bin, sep = "_")
  )

# Calculate richness and record count per cell
grid_stats <- occurrences %>%
  group_by(cell_id) %>%
  summarize(
    n_records = n(),
    n_species = n_distinct(species)
  )
# Filter cells with at least 20 records
grid_stats <- filter(grid_stats, n_records >= 20)
```

In this snippet, we binned lat/long into 0.5° cells (you could also use a fixed coordinate grid or a shapefile of equal-area grids). Then we counted how many records and how many species each cell has, and filtered to cells with 20+ records to focus on reasonably sampled cells. The result `grid_stats` can be joined to an `sf` grid for mapping. This kind of map will still show some bias (20 records in one cell might catalog fewer species than 20 in another if communities differ), but it’s much better than mapping raw occurrence counts. **The key is that every mapped cell is based on comparable effort.**

For an even more robust approach, you could calculate a rarefied richness per cell. This essentially combines the above two methods: use each cell’s species-occurrence data to compute an ESThreshold (like ES20 if 20 is the minimum records per cell, or ES50 if many cells have ≥50 records). Cells with fewer records than the threshold can be ignored or shown as having lower confidence. The result is a map like the one above, where sampling bias is greatly reduced and you can start to see the true patterns of biodiversity.

## Putting It All Together (and Final Tips)

Handling sampling bias often requires **using multiple methods in tandem** and making judgment calls based on your data and research question. There is no one-size-fits-all fix, but the approaches above are commonly used toolbox items:

* **Spatial thinning** removes oversampled clusters and is great for preparing data for species distribution modeling or anytime spatial autocorrelation from sampling is a concern.
* **Effort filtering** ensures you’re comparing data with similar collection effort (filtering out the junk or overly duplicated observations), which yields more reliable analyses.
* **Rarefaction** standardizes comparisons by equalizing sample sizes, crucial for fair biodiversity comparisons across regions or time periods.
* **Grid-based estimation** provides a structured way to aggregate data spatially, often combined with minimum effort thresholds or rarefaction to produce maps that reflect reality more than collector density.

In practice, you might apply **several of these in one workflow**. For example, if you were studying the diversity of butterflies in South America, you could first filter out records without good effort info (say, only use survey data, or only one record per site/day), then apply spatial thinning so that e.g. no two records are within 10 km. Next, divide the continent into grid cells and compute rarefied species richness per cell. The end result could be a richness map that is much less biased. You’d avoid the pitfall of simply mapping raw data which might have all of coastal Brazil glowing just because that’s where people live and collect data, and instead highlight inland Amazon cells if the data – once equalized – show high richness there.

Finally, always remember to **check the sensitivity** of your results to these methods. Because each method involves discarding or re-weighting data, it’s good to try a few variations (e.g. different thinning distances or rarefaction sizes) to ensure your conclusions aren’t an artifact of the correction method. The improvements are usually worth it: accounting for sampling bias can lead to major gains in the realism and accuracy of your analyses, even if it means working with a bit less data. In summary, by applying these techniques, you’ll extract more meaningful ecological signal from the noise of sampling bias – turning a biased occurrence dataset into insights that are closer to the truth of nature’s patterns. Good luck, and happy (unbiased) mapping!

**Sources:**

* Waller, J. (2019). *Exploring es50 for GBIF*. GBIF Data Blog.
* GBIF (2023). *Sampling biases shape our view of the natural world*.
* Plantarum (2021). *Thinning Occurrence Records in R*.
* Best Practices for Using eBird Data (2021). *Complete Checklists*.
* Meyer et al. (2016). *Spatial bias in the GBIF database affects SDMs*.


<!--more-->
