# Application: Movie Rating Prediction
#### Experiments for Data Civilizer pipelines

-----

The goal is to **predict movie ratings** of a generic audience (i.e., *imdb.com* users), given the **critic ratings** and other features (e.g., genre, year) gathered from *rogerebert.com* (a website collecting reviews by critics)

The two employed data sets have the following characteristics:
    
|dataset|#records|notes|
|:--: |:--:|:--:|
|Roger Ebert (https://www.rogerebert.com) |3.5k|contains information about critic rating|
|IMBD (https://www.imdb.com) |6.8k|contains information about audience rating|


**Labeled data for DeepER**:

For running DeepER, labeled data has been provided by manually labeling 370 manually pairs of records (200 of which are matches).

(When trained, DeepER finds 350 matches).


-----

### LinerRegression for predicting the rating

A *linear regression* model is trained for predicting the "*critic score*" (i.e., the target).
The train/test set are 75%/25% of the data set.

For assessing the quality of the cleaning pipeline, the following pipeline variations are considered:

1. Only Entity Resolution is performed: DeepER is applied to find matches between the two datasets, then the records are clustered consequently.

2. Fahes is employed to discover DMVs, which are set to NULL; then Entity Resolution is performed.

3. Fahes is employed to discover DMVs, which are set to NULL; ImputeDB is employed for the imputation of the NULL values; then Entity Resolution is performed.

4. Fahes is employed to discover DMVs, which are set to NULL; ImputeDB is employed for the imputation of the NULL values; then Entity Resolution is performed and refined manually (expressing simple handcrafted rules on the data).

Notes:

- *The Golden Record is not working well with this data set; when common attributes are present, only the values from rogertebert.com are kept*.
- Discovered DMVs have been validated by the user to avoid false positive.

---


**Features**:

- **list**:
	- *critic_rating*,
	- *pg_rating*, (suitable for children, < 13yo, < 17yo, etc.)
	- *year*,
	- *duration*,
	- *actors*,
	- *movie_rating*

- **One hot encoding** has been employed w/ categorical features

---

**Metrics**: r squared ([r^2](https://en.wikipedia.org/wiki/Coefficient_of_determination)) is employed for assessing the quality of the predictions.
The ideal method reaches 1.

---

### Results

||Pipeline|**r^2**|
|:--:|:--:|:--:|
|1| DeepER| <span style="color:#557799">**0.31**</span> |
|2| Fahes -> set NULL -> DeepER | <span style="color:#AA5555">**0.21**</span> |
|3| Fahes -> ImputeDB -> DeepER      | <span style="color:#0055BB">**0.36**</span> |
|4| Fahes -> ImputeDB -> DeepER -> Manual rules for Entity Matching | <span style="color:#0022FF">**0.52**</span> |


**notes**:

- Entities that need manual rules:
	
	- similar movies:
	  
	  - *Rush Hour 2* != *Rush Hour 3*
	
	- same title different year and director:
	  
	  - *Rage [year: 2014]* != *Rage [year: 2010]* != *Rage [year: 1995]*

- Using only *critic_rating* for predicting *movie_rating*, lower r^2 are always achieved

- Using other features but *critic_rating*, r^2 is always very low or negative (i.e., meaningless predictions).

---

Other data sets: [Magellan data repository](https://sites.google.com/site/anhaidgroup/useful-stuff/data)

