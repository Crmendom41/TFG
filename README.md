# TFG
This repository includes the code generated throughout the development of the project: a Jupyter Notebook (Python) used to create the final database and an R markdown file used to perform the statistical analysis.


### Contents
- [Database creation (Python)](#database_creation_(Python))
- [Statistical analysis (R)](#statistical_analysis_(R))

## Database creation (Python)
The jupyter notebook ```bbdd_final_eng.ipynb``` is used to create the final database that will be used to perform the statistical analysis. It should be considered that the final database will be the result of concatenating the database with the clinical variables and the measurements of the tricuspid valve, and the database generated from the atrial remodeling data obtained from ADAS 3D. 

### ADAS 3D database creation
But before proceeding to merge the final database, the data regarding atrial remodeling obtained from ADAS 3D needs to be structured into a database. The data downloaded has the form of many CSV files stored in two different folders, one for the right atrium (RA) and one for the left atrium (LA). 

Thus, all the variables inside these CSV files of each of the patients need to be placed in a dataframe containing the variables in the columns and the patients in the rows. By running a loop over the patients folders and each of the patients' heart chambers, the CSV files are opened and converted into a single row of the dataframe. For this purpose, a function called ```csv_to_df``` has been defined. 

### Creation of new features: atrial remodeling
The data contained in the database generated with the ```csv_to_df``` function is the one that has directly been downloaded from the 3D model generated with ADAS 3D. However, the features needed for the statistical analysis corresponding to the body of the chambers (subtracting the veins, valves and appendages from the complete chamber) need to be computed. 

In the case of atrial structural remodeling (fibrosis), for each of the chambers, there are features corresponding to the area of fibrosis, in cm2, and to the percentage of fibrosis, in %, of the total chamber and the different structures (veins, valves and appendages). The variables needed for the statistical analysis are not these but the ones corresponding to the body of the chambers, which are computed by subtracting the values of each structure from the total of the chamber.

The features corresponding to the morphological remodeling of the atria are already computed in the downloaded CSV files. These are the volumes of each chamber and the sphericity.

### Creation of new features: LA regions
Despite this is not included in the project scope, for future analyses, the variables corresponding to the structural remodeling of the left atria had to be computed by regions.

### Concatenation of the two databases
Once the database has been created from the ADAS 3D features in the form of a dataframe, the dataset containing the clinical and TA variables is opened as a dataframe. Previously, the excel file has been saved into a CSV file so that it can be opened in the code.

Then, the two dataframes are concatenated together horizontally so that each row corresponds to a patient and the columns contain all the variables. Before merging the two dataframes, a new column corresponding to the Eccentricity Index (EI) of the tricuspid annulus (TA) is created as the ratio between the short and long axes of the TA.

Finally, the dataframes are concatenated and saved in the form of a CSV file.



## Statistical analysis (R)
The statistical analysis performed for this project consists of simple logistic and linear regressions presented in the file ```Tricuspide_edit.Rmd```. 

It has been done using an R markdown script and the process has consisted in the following:
- Calculate the predictors of the binary outcome (severity_TR_eco) using Logistic Regression.
- Calculate the predictors of the continuous outcomes (AA_PRE_ADAS_mm2, PA_PRE_ADAS_mm, DMAX_PRE_ADAS_mm, DMIN_PRE_ADASmm and Eccentricity_1) through the use of Linear Regressions.
- Use ggplot2 library to display some plots of the most significant results. 
