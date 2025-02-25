Our [Run Models CoLab](input/industries) provides Logistic Regression, Support Vector Machines (SVM), MLP, RandomForest, XGBoost 

Our main input is currently industry features by county for exploring environmental impact targets.  
We are also creating [CoLabs for Exiobase International Trade Flow](https://model.earth/profile/trade).


[Run-Models-bkup.ipynb](https://github.com/ModelEarth/RealityStream/tree/main/models) is a backup of the [Run Models CoLab](https://colab.research.google.com/drive/1zu0WcCiIJ5X3iN1Hd1KSW4dGn0JuodB8?usp=sharing) that we run locally. We append "-bkup" to indicate it is not the primary source.

<h2>Design your Stream</h2>

Select a default yaml file. First one (and possibly blinks) is ready to use:  
[parameters-simple.yaml](https://raw.githubusercontent.com/ModelEarth/RealityStream/main/parameters/parameters-simple.yaml) - TO DO: Test this simple version and modify CoLab if needed (no years, just Maine)
[parameters.yaml](https://raw.githubusercontent.com/ModelEarth/RealityStream/main/parameters/parameters.yaml) - Predicts bee population growth by industry  
[parameters-years.yaml](https://raw.githubusercontent.com/ModelEarth/RealityStream/main/parameters/parameters-years.yaml) - For testing with multiple years and states (currently same as parameters.yaml)
[parameters-zip.yaml](https://raw.githubusercontent.com/ModelEarth/RealityStream/main/parameters/parameters-zip.yaml) - Needs zip code target  
[parameters-blinks.yaml](https://raw.githubusercontent.com/ModelEarth/RealityStream/main/parameters/parameters-blinks.yaml) - Uses only features dataset (which contains the target column)

