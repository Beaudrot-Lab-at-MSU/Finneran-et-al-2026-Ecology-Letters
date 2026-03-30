# Food web similarity increases with productivity similarity at a continental scale

Dataset DOI: [10.5061/dryad.zkh1893nh](https://doi.org/10.5061/dryad.zkh1893nh)
Link to Paper: https://onlinelibrary.wiley.com/doi/10.1111/ele.70368

License: CC BY-NC 4.0

## Description of the data and file structure

This dataset includes the data and code required to replicate analyses in this manuscript, examining the environmental drivers of food web structure across 127 protected areas in Sub-Saharan Africa. Data includes meta-web predator-prey interaction data for non-volant mammal species larger than 500g in accordance with Jonathan Kingdon's Mammals of Africa volumes and with published global predator-prey interaction data (Fricke *et al.* 2022). Coordinates correspond to protected-area centroids and do not represent precise species occurrence locations.

We also grouped these 127 sites into Bioregions in accordance with One Earth Bioregions ([https://www.oneearth.org/bioregions](https://www.oneearth.org/bioregions)). We used the realm and sub-realm designations to group sites with similar habitats within the same realm or sub-realm, which resulted in 6 regions. Please request GIS files from One Earth directly per their request, [https://www.oneearth.org/contactus ](https://www.oneearth.org/contactus)

To quantify pairwise differences among sites in food web structure, we used a network dissimilarity measure from optimal transport theory and transformed this to a measure of similarity.

We also gathered data on primary productivity and anthropogenic fragmentation. We used Google Earth Engine at a 500m resolution for a 10km buffer around the published geographic coordinates for each site.

We tested the relative roles of primary productivity and fragmentation on food web similarity at two spatial scales: 1) all African sites and 2) sites within each biogeographic region. Specifically, we used the “mantel” function from the ecodist R package with the Pearson method and 10,000 permutations. We also included geographic proximity to account for spatial autocorrelation.

### Steps to run analysis: Download all files to the same location on your computer.

#### **Step 1:** *Run the GEE scripts in Google Earth Engine*. The output of this code will be a CSV with the primary productivity values and a collection of tif files to be used in the next step for calculating the fragmentation at each site.

One must create a google earth engine (GEE) profile in order to run these scripts, [https://earthengine.google.com](https://earthengine.google.com/), and use the code editor [https://code.earthengine.google.com](https://code.earthengine.google.com). The language of GEE is Java. Upload the shapefiles to your legacy assets, and copy and paste the text from the Word documents in 2 separate scripts in GEE.  You will need to add your username to the featurecollection line with the x. It may take a few minutes for the code to run. The files will be saved to your Google Drive.

Files for this are: **a)** GEE_Fragmentation_Code **b)** GEE_EVI_Code **c)** Shape_Files_GEE code files

##### File: GEE_EVI_Code.txt

**Description:** This JavaScript is used to calculate the mean values of vegetation indices from a large amount of raster layers as collection on Google Earth Engine (GEE). Note that a GEE requires an account and may need to be linked with a university.  Paste this code into a script in the code editor of GEE. This script also requires the shape files from the Shape_Files_GEE.zip to denote which sites GEE should extract data from.

##### File: GEE_Fragmentation_Code.txt

**Description:** This JavaScript is used to calculate the values of anthropogenic fragmentation on Google Earth Engine (GEE). Note that a GEE requires an account and may need to be linked with a university. Paste this code into a script in the code editor of GEE. This script also requires the shape files from the Shape_Files_GEE.zip to denote which sites GEE should extract data from.

##### File: Shape_Files_GEE.zip

**Description:** This zip file contains shape files to import into GEE to extract the environmental variables for each site. This shapefile was created in R and QGIS from the coordinates for each site.

#### **Step 2:** *Run the Fragmentation.qmd file*. The output of this will be the csv with the fragmentation values to use in later analyses. 

Files for this are: **a)** African_Mammal_FoodWebs_Locations.csv **b)** AfricanMammalCommunityLandCoverTIFFS_BI folder unzipped **c).** Fragmentation.qmd

##### File: AfricanMammalCommunityLandCoverTIFFS_BI.zip

**Description**: This is a zipped folder of all the TIFF files from the GEE code. Download and unzip this to continue the fragmentation analysis.

##### File: Fragmentation.qmd

**Description**: This is a quarto file to open in R to calculate the fragmentation metric used in this paper from the TIFF files derived from GEE.

##### File: African_Mammal_FoodWebs_Locations.csv

**Description:** location of each site, Code name, and full name, from Rowan, J., et al (2020). Please note that coordinates throughout this project correspond to protected-area centroids and do not represent precise species occurrence locations.

*Variables*

* Community: shortened name for site
* Full Name: full name for community
* Country: the country site is found in
* Long: longitude of coordinate
* Lat: latitude of coordinate
* Code: code we gave to each site
* Remove: 0 to retain and 1 to remove if user wishes to only use subset of sites

#### **Step 3**: *Run the FGW Code in python*. The output of this will be the FGW distances between each site to be used in later analyses. If you are interested, more info about this distance can be found in this article: [https://besjournals.onlinelibrary.wiley.com/doi/10.1111/2041-210x.70130](https://besjournals.onlinelibrary.wiley.com/doi/10.1111/2041-210x.70130)

Files for this are: **a)** Common_Names_Mammals.csv **b)** compute_fused_gw.ipynb **c)** functional_traits.csv **d)** fused_gw._env.yml **e)** got.py **f)** above_500_PA_data.csv **g)** Final_Regions_and_Predictors.csv **h)** Africa_metaweb_above500.csv

`got.py` is a package that carries all the functionalities to perform optimal transport on graphs, specifically the Gromov-Wasserstein and Fused Gromov-Wasserstein. For first-time users, we recommend the lightweight miniconda distribution that can be found [here](https://docs.anaconda.com/free/miniconda/). If the user is on Mac, consider using [Homebrew](https://brew.sh/) to [install Miniconda](https://formulae.brew.sh/cask/miniconda).

After conda is installed on your device, create a virtual environment from environment.yml by running in the terminal: 

conda env create -f fused_gw._env.yml -p ./envs

which should create a folder in the top-level directory named envs with all the packages you need. Change your directory as needed to ensure that in the terminal, you are in the directory where you downloaded the files.

To activate this virtual environment, you can then run the following command: 
conda activate ./envs

Next, you simply run: 
jupyter notebook

and select compute_fused_gw.ipynb 

##### File: Common_Names_Mammals.csv

**Description**: csv of common names to make examination of FGW and webs more intuitive than using the scientific name

*Variables*

* Order: order of species
* Family: family of species
* Genus: genus of species
* Species: scientific name
* Common name: common names of the species

##### File: compute_fused_gw.ipynb

**Description:** Jupyter Notebook file for calculating FGW distances.

##### File: functional_traits.csv

**Description:** functional traits used to calculate the fused aspect of the fused Gromov-Wasserstein distance (FGW). This information is from [Kissling et al., 2014 Ecology and Evolution.](https://onlinelibrary.wiley.com/doi/full/10.1002/ece3.1136)

*Variables*

* Species: species name
* Mass: in grams
* All subsequent columns are the name of a resource a species may consume (Vertebrate, Plant, Invertebrate, etc.). Each entry has a number from 0-3.  0=this species does not consume this resource. 1-3 were categorical variables relating to data in  Kissling et al article, but in this manuscript, 1-3 were all categorized as a 1.

##### File: fused_gw._env.yml

**Description:** This is the file to create a virtual environment to run the Jupyter Notebook file.

##### File: got.py

**Description:** This is a package that carries all the functionalities to perform optimal transport on graphs, specifically the Gromov-Wasserstein and Fused Gromov-Wasserstein

##### File: above_500_PA_data.csv

**Description:** presence/absence data for each site, from Rowan, J., et al (2020).

*Variables*

* Rows are species and columns are site Codes
* 0 denotes the species is not there, 1 denotes the species is there

##### File: Africa_metaweb_above500.csv

**Description:** Predator-prey interactions for mammal species in Sub-Saharan Africa. Columns denote predator species, and rows denote prey. Numbers 4,5 and 6 denote interactions added after further investigation. In our analyses, numbers 1-6 collapse into just 1s to denote that an interaction occurs.

*Values*

* 0: predator does not consume prey
* 1 or 4: the species had been documented as prey of the predator
* 2 or 5: the species’ genus, but not the species itself, had been documented as prey of the predator
* 3 or 6: the body size of the species fell within the body size range of documented prey of the predator or the documented range of acceptable prey body mass for the predator

##### File: Final_Regions_and_Predictors.csv

**Description:** Contains site data and environmental predictor data for each site.

*Variables*

* Code: code we gave to each site
* Region: Region we created from One Earth Bioregions
* Community: shortened name for site
* Full Name: full name for community
* Country: the country site is found in
* Long: longitude of coordinate
* Lat: latitude of coordinate
* BioregionName: One Earth Bioregion that the site is located in
* SubRealm: One Earth SubRealm that the site is located in
* num_nodes: number of nodes (species) in the food web at a site
* EVI_Mean: average EVI value for the site from GEE.
* bin.AG.class: binary aggregation index at the class level. This can be calculated in the Fragmentation.qmd file and was added to this file manually.
* EVI_std: standard deviation of EVI over the time period from GEE
* Frag_std: standard deviation of the binary aggregation index over the time period from GEE
* Remove: 0 to retain and 1 to remove if user wishes to only use subset of predictors

##### **Step 4:** *Run the Manuscript_Figures_Similarity.qmd file in R Studio*. The output of this file will create all figures and results mentioned in the manuscript and supplement.

Please read the file carefully as there is a common bug with the ecodist package that is discussed in the file as well as troubleshooting instructions. Do NOT click "RUN ALL".  You may need to restart R and clear all output to ensure the ecodist() package is loaded correctly and not overwritten by another package. The file contains a line to check if the package loaded correctly.

Files for this are: **a)** Manuscript_Figures_Similarity.qmd **b)** NetworkMetrics.csv **c)** Partial_Mantel_Barplot.csv **d)** Node_Data_500g.csv **e)** Africa_metaweb_above500.csv **f)** African_Mammal_FoodWebs_Locations.csv **g)** Final_Regions_and_Predictors.csv **h)** pairwise_fgw.csv **i)** above_500_PA_data.csv

##### File: Manuscript_Figures_Similarity.qmd

**Description:** This **quarto file for R has the main code needed to replicate figures and results** in this manuscript and supplement in an easy-to-follow format. This file can be opened in R Studio after being downloaded from Dryad. RStudio should prompt users to install Quarto automatically when opening this file.

##### File: NetworkMetrics.csv

**Description:** This file is used in the appendix when discussing why we logged fGW to account for relationships between the network metrics of connectance and species richness. Connectance can be calculated simply in R.

*Variables*

* Code: site
* Connectance: connectance
* Species_Richness: number of nodes
* Remove:  "0" to retain and "1" to remove. All sites were retained in the manuscript analyses.

##### File: Partial_Mantel_Barplot.csv

**Description:** Results from partial Mantel analysis in the correct format for recreating barplot. This file was created by running the function in the qmd file above for each region and for all sites overall, and manually creating the table.

*Variables*

* Region: the Bioregion 
* Predictor: EVI or Fragmentation
* r: the partial Mantel correlation coefficient
* p: the p-value

##### File: Node_Data_500g.csv

**Description:** Information on species taxonomy and mass for visualizing the meta-web better.

*Variables*

* Species: scientific name
* Order: order of species
* Family: family of species
* Genus: genus of species
* Mass.g: mass of species in grams

##### File: pairwise_fgw.csv

**Description:** fused Gromov-Wasserstein values between each site in matrix format. This is calculated in the Python script.

## Code/software

Google Earth Engine is needed to gather new environmental data with a JavaScript. R Studio is needed to access the .qmd files. Python is needed to calculate the FGW distances (more specific instructions are given for running this analysis in a virtual environment).

## Access information

Other publicly accessible locations of the data (FGW):

* [https://zenodo.org/records/16103567](https://zenodo.org/records/16103567)

Data was derived from the following sources:

* Rowan, J., Beaudrot, L., Franklin, J., Reed, K. E., Smail, I. E., Zamora, A., & Kamilar, J. M. (2020). Geographically divergent evolutionary and ecological legacies shape mammal biodiversity in the global tropics and subtropics. Proceedings of the National Academy of Sciences, 117(3), 1559–1565. [https://doi.org/10.1073/pnas.1910489116](https://doi.org/10.1073/pnas.1910489116)
* Kingdon, J., Happold, D., Butynski, T., Hoffmann, M., Happold, M., & Kalina, J. (2013). *Mammals of Africa*. Bloomsbury Publishing. [https://books.google.com/books?id=B\\\\_07noCPc4kC](https://books.google.com/books?id=B\\\\_07noCPc4kC)
* Fricke, E.C., Hsieh, C., Middleton, O., Gorczynski, D., Cappello, C.D., Sanisidro, O., *et al.* (2022). Collapse of terrestrial mammal food webs since the Late Pleistocene. *Science*, 377, 1008–1011.
* Kissling, W. D., Dalby, L., Fløjgaard, C., Lenoir, J., Sandel, B., Sandom, C., Trøjelsgaard, K., & Svenning, J.-C. (2014). Establishing macroecological trait datasets: digitalization, extrapolation, and validation of diet preferences in terrestrial mammals worldwide. *Ecology and Evolution*, *4*(14), 2913–2930. DOI: 10.1002/ece3.1136. 
