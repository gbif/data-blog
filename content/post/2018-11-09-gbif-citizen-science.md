---
title: Finding citizen science datasets on GBIF
author: Marie Grosjean
date: '2018-11-09'
slug: gbif-citizen-science
categories:
  - GBIF
tags:
  - gbif
  - citizen
  - dataset
  - API
lastmod: '2018-11-09'
keywords: ['Citizen Science', 'prediction', 'machine learning']
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

# Can we automatically label citizen science datasets?

The short answer is yes, partially.

## Why label GBIF datasets as "citizen science"?

### What is citizen science?

Citizen science is scientific research conducted, in whole or in part, by amateur (or non-professional) scientists. Citizen science is sometimes described as "public participation in scientific research," participatory monitoring, and participatory action research ([wikipedia definition](https://en.wikipedia.org/wiki/Citizen_science)).

### Citizen science on GBIF

[A 2016 study](https://doi.org/10.1016/j.biocon.2016.09.004) showed that nearly half of all occurrence records shared through the GBIF network come from datasets with significant volunteer contributions (for more information, see our ["citizen science" page](https://www.gbif.org/citizen-science) on gbif.org).

We would like to know what is the proportion of citizen science dataset now, two years later.

## My analysis

### The tools I used

I wrote my script in python, using the [textblob](https://textblob.readthedocs.io/en/dev/) library and the [GBIF API](https://www.gbif.org/developer/summary).

I found some inspiration here:

* https://www.analyticsvidhya.com/blog/2018/02/the-different-methods-deal-text-data-predictive-python/
* https://stevenloria.com/simple-text-classification/

My script, training set and results are available on [GitHub](https://github.com/ManonGros/Small-scripts-using-GBIF-API/tree/master/find_citizen_science_datasets).

### My training and testing set

Before starting my analysis, I assembled a training and testing set. I included all the datasets used to write [this publication](https://doi.org/10.1016/j.biocon.2016.09.004) and some datasets I annotated manually.

The criteria I used to decide what I would label as citizen science (CS) are the following:

* Metadata explicitly includes the words "citizen" or "citizen science" (or for the French version "science" or "enquête" participative)
* The metadata mentions that the dataset is partly or entirely made by volunteers
* Bioblitz datasets
* Known citizen science project (Tela-botanica, for example)

What I **did not** consider as citizen science was:

* What seems like compulsory student work
* Personal collections or notebook (unless the description includes clue from above)

My training/testing set contains 226 CS datasets and 788 non-CS datasets. This set is available on[GitHub](https://github.com/ManonGros/Small-scripts-using-GBIF-API/blob/master/find_citizen_science_datasets/some_manually_annotated_datasets.tsv).

### Obtained and processed the data

The code I wrote for everything below is available on [GitHub](https://github.com/ManonGros/Small-scripts-using-GBIF-API/blob/master/find_citizen_science_datasets/scripts/citizen_science_gather_dataset_descriptions.ipynb).

### Title, description, keywords, methods and additional data

As mentioned above, I used the [GBIF API](https://www.gbif.org/developer/summary) to get the datasets' descriptions. Since I would like to predict CS datasets using only datasets' metadata, I chose to include:

* The title of the dataset
* The contain of its description box
* Its keywords
* Its method section if any
* Anything contained in the "additional information" category

I then concatenate this into a long string of character.

For example, [this  dataset](https://www.gbif.org/dataset/172149d2-2dc0-43fb-a7b1-57e3e4ec34a2) titled "Powdery Mildew Citizen Science Survey, UK, 2013-2015" would result into:

> Powdery Mildew Citizen Science Survey, UK, 2013-2015 The distribution and identity of powdery mildew species samples sent to the University of Reading for the citizen science survey run between 2013 and 2016 The dataset has been shecked using the NBN Record Cleaner. Samples of this fungal plant disease were collected with their host plants randomly by members of the public. Samples were sent by post to the project convenor at the University of Reading, funded by the Royal Horticultural Society, who identified the different powdery mildew species. 
Powdery mildews are traditionally identified using a combination of their appearance and the identification of their host plant.  Some plants have only one known powdery mildew so identification of the host allows an immediate tentative identification of the pathogen, while others have several known mildew pathogens. Using a light microscope morphological features of the mildew must be found and the mildew can then be identified using a key and by comparison with published images. In many cases these features can be difficult to distinguish between species or can only be found at a certain stage of the life cycle of the fungus. Molecular DNA extraction and sequencing then completes the process as sequences are matched to a database of known powdery mildews. 


 NB: I didn't include datasets from [PLAZI](https://www.gbif.org/publisher/7ce8aef0-9e92-11dc-8738-b8a03c50a862) and [PANGAEA](https://www.gbif.org/publisher/d5778510-eb28-11da-8629-b8a03c50a862). Nor did I include checklists (check data classes [here](https://www.gbif.org/dataset-classes) for more information).

### Process the data

We would like to create a "bag of words" for each datasets, preferably with only informative words. For that, we will go through a few transformation steps.

### 1) Set everything to lower case, remove punctuation, numbers, hyphens and underscores.

Our example would now look like:

>powdery mildew citizen science survey uk   the distribution and identity of powdery mildew species samples sent to the university of reading for the citizen science survey run between  and  the dataset has been shecked using the nbn record cleaner samples of this fungal plant disease were collected with their host plants randomly by members of the public samples were sent by post to the project convenor at the university of reading funded by the royal horticultural society who identified the different powdery mildew species powdery mildews are traditionally identified using a combination of their appearance and the identification of their host plant some plants have only one known powdery mildew so identification of the host allows an immediate tentative identification of the pathogen while others have several known mildew pathogens using a light microscope morphological features of the mildew must be found and the mildew can then be identified using a key and by comparison with published images in many cases these features can be difficult to distinguish between species or can only be found at a certain stage of the life cycle of the fungus molecular dna extraction and sequencing then completes the process as sequences are matched to a database of known powdery mildews


### 2) Remove most frequent words

They are unlikely to be informative and will be a lot more to process in the analysis. I decided to remove the most frequent words by language to avoid language bias. We don't want words like "como" to be predictive just because the training set doesn't contain a lot of Spanish description for citizen science projects.

I chose the arbitrary threshold of removing the 15 most common words for each language. Note that I didn't optimize this threshold.

Our example would now be:

> powdery mildew citizen science survey uk distribution identity powdery mildew samples sent university reading citizen science survey run between dataset has been shecked nbn record cleaner samples fungal plant disease collected with their host randomly members public samples sent post project convenor at university reading funded royal horticultural society who identified different powdery mildew powdery mildews traditionally identified combination their appearance identification their host plant some have only one known powdery mildew so identification host allows an immediate tentative identification pathogen while others have several known mildew pathogens light microscope morphological features mildew must be found mildew can then be identified key comparison with published images many cases these features can be difficult distinguish between or can only be found at certain stage life cycle fungus molecular dna extraction sequencing then completes process sequences matched known powdery mildews


### 3) Replace and remove some key words

This step involves mostly manual curation. This is partly how I get around the multi-language problem. I simply remove or translate a list of words I curated manually.

This list is accessible on [GitHub](https://github.com/ManonGros/Small-scripts-using-GBIF-API/blob/master/find_citizen_science_datasets/wd_replace.txt).

After this step, our example would still be the same.

**About languages**
I tried using the translation function in Textblob and it worked! For about an hour. After that the Google API didn't respond any more so I gave up. If anyone has a better method to deal with multi-language analysis, please leave a comment. I would be very grateful.

This is the language distribution for the GBIF datasets (excluding PLAZI and PANGAEA's):
```
eng    6591
spa    1757
fra    1114
por     223
nor       6
zho       6
rus       2
glg       1
cat       1
 ```


### 4) Remove rare words - optional

For the same reason we removed frequent words, we would also like to remove rare words (which I defined as occurring only once).
However, there were so many rare words that the running time was very long so I decided to keep them (the code is available though).

### 5) Correct spelling - optional

I gave up on correcting the spelling mistakes because it didn't make any sense for non-English datasets but the code is available.

### 6) Lemmatization

Lemmatization is the process of "making words more universal". For example, the word "butterflies" would become "butterfly", etc.

Our lemmatized description would be:

> powdery mildew citizen science survey uk distribution identity powdery mildew sample sent university reading citizen science survey run between dataset ha been shecked nbn record cleaner sample fungal plant disease collected with their host randomly member public sample sent post project convenor at university reading funded royal horticultural society who identified different powdery mildew powdery mildew traditionally identified combination their appearance identification their host plant some have only one known powdery mildew so identification host allows an immediate tentative identification pathogen while others have several known mildew pathogen light microscope morphological feature mildew must be found mildew can then be identified key comparison with published image many case these feature can be difficult distinguish between or can only be found at certain stage life cycle fungus molecular dna extraction sequencing then completes process sequence matched known powdery mildew


### Feature selection

The feature selection is simply the process by which I select informative features. In other words: what words are important to label the datasets as CS?
I would like a small set of informative words to avoid over-fitting my training set.

The proper way to do this is to use a double loop cross-validation but I don't have enough data for that. So instead, I chose the following arbitrary method:


1. Selected 800 random datasets as the "training set".
2. Divide this training set in four subsets.
3. For each subset:
	 3.1  train a Naive Bayesian model,
	 3.2  extract the top 15 informative words.
4.  Keep only the informative words that were selected more than once.


The resulting list of informative words varies in length depending on the random selection but the most important ones are fairly consistent.
In my latest iteration, the resulting list of features was the following:
```
na              3
select          3
professional    3
occurence       3
school          3
validated       3
organised       2
amateur         2
dedicated       2
spot            2
you             2
citizen         2
hosted          2
volunteer       2
```

### Model training

I then stripped my training/testing set of all the words that weren't informative. I kept only the list above + `museum` + `herbarium` + `inaturalist` because I thought that it would improve the outcome (but I was wrong, it didn't change anything).

Our example became:

> citizen citizen

I know, I should use a completely new training set to re-train the final model but I don't have enough data. So instead, I trained a decision tree on my previous 800 datasets (the ones I used for feature selection).

I chose a decision tree model this time, because it is easier to interpret and implement.

This is the resulting truncated tree written as pseudo-code:

```
if contains(citizen) == False: 
  if contains(volunteer) == False: 
    if contains(organised) == False: 
      if contains(amateur) == False: return 'F'
      if contains(amateur) == True: return 'T'
    if contains(organised) == True: 
      if contains(na) == False: return 'T'
      if contains(na) == True: return 'T'
  if contains(volunteer) == True: return 'T'
if contains(citizen) == True: return 'T'
```

### Test and performance

I tested this tree on the testing set (which was never used for the feature selection nor the training).
This testing set contained 21 CS and 121 non-CS.

The performance on this test set was:
```
Performance on testing set

True positive:	 9.86 %
True negative:	 85.21%
False positive:	 4.93 %
False negative:	 0.0  %
-----------------------------------
Accuracy:	 95.07%
```

It is clearly over estimated. I ran the feature selection and training a few times and overall, the accuracy is between **83** and **95%**.
Every time I ran my script, I got more false positives than false negatives.


### Results

The result of the model prediction for the unclassified datasets is available on [GitHub](https://github.com/ManonGros/Small-scripts-using-GBIF-API/blob/master/find_citizen_science_datasets/test_model_subsample.tsv).

According to my model, about more 200 datasets can be labeled as "citizen science" (in addition to the 226 I manually annotated), which corresponds to 1% of the total number of datasets on GBIF.

This number is probably greatly overestimated since the model gives us more false positive than false negative. Nevertheless, it is a relatively accurate estimation.

**Edit 2018-11-27**: I checked the predictions and made a list of citizen science datasets on GBIF available on [GitHub](https://github.com/ManonGros/Small-scripts-using-GBIF-API/blob/master/find_citizen_science_datasets/citizen_science_datasets_26nove2018_not_reviewed.tsv). I also tagged these datasets in the API using a machineTag. You can now look for them with the following call: http://api.gbif.org/v1/dataset?machineTagNamespace=citizenScience.mgrosjean.gbif.org.
If you don't agree with any of these annotations, please let me know and I will update the tags.

### My conclusion

I tried to keep my model as simple as possible. Mostly because having a complicated and uninterpretable model wouldn't be very useful for me.

This means that, perhaps, I could have come up with this simple model myself (without having to use any machine learning). Especially since this model would require manual curation afterwards.

However, my script has two advantages:

* It transforms the input (mostly useful for non-English languages)
* It evaluates the model performance.

### What do you think? 
Should GBIF explore a system like this to label datasets as "citizen science"? 
Please let me know.
