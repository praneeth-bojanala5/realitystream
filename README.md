Our [Run Models CoLab](input/industries) provides Logistic Regression, Support Vector Machines (SVM), MLP, RandomForest, XGBoost.

Our main input is currently industry features by county for exploring environmental impact targets.  
We are also creating [CoLabs for Exiobase International Trade Flow](https://model.earth/profile/trade).


[Run-Models-bkup.ipynb](https://github.com/ModelEarth/realitystream/tree/main/models) is a backup of the [Run Models CoLab](https://colab.research.google.com/drive/1zu0WcCiIJ5X3iN1Hd1KSW4dGn0JuodB8?usp=sharing) that we run locally. We append "-bkup" to indicate it is not the primary source.

<h2>Design your Stream</h2>

**Bee YAML Updated** - Changed [bee data](input/bees) target to bees-targets-top-20-percent.csv in parameters yaml. This new "colony density" target uses the top 20% of counties with the highest bee population density (rather than top colony growth between years, as was used in bees-targets.csv).

<!--
Density file: bees-targets-top-20-percent.csv. Shashank worked from bees-population-usda.csv
(We previously used growth over time with the file bees-targets.csv)
-->

Copy and paste one of the following or use the CoLabs default settings:
[parameters-simple.yaml](https://raw.githubusercontent.com/ModelEarth/realitystream/main/parameters/parameters-simple.yaml) - 2020, just Maine
[parameters.yaml](https://raw.githubusercontent.com/ModelEarth/realitystream/main/parameters/parameters.yaml) - Predicts bee density by industry  
[parameters-years.yaml](https://raw.githubusercontent.com/ModelEarth/realitystream/main/parameters/parameters-years.yaml) - For testing with multiple years and states (currently same as parameters.yaml).  Uses bee populatin growth.
[parameters-zip.yaml](https://raw.githubusercontent.com/ModelEarth/realitystream/main/parameters/parameters-zip.yaml) - Needs zip code target. Uses bee populatin growth.  
[parameters-blinks.yaml](https://raw.githubusercontent.com/ModelEarth/realitystream/main/parameters/parameters-blinks.yaml) - Uses only features dataset (which contains the target column).

