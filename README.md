# NBA PLAYER ARCHETYPES VISUALIZATION

## 1) Description

This software package is designed to build off the 2022 Muniz and Tulay approach of creating a weighted network of NBA players in the Journal of Sports Analytics. It works as follows:

### a) Datasets

There are six master datasets used in this program. These datasets use the nba_api to form per game averaged pace adjusted csv files for each player between a set of specified seasons which represent the players scoring, passing/playmaking, defence, rebounding, clutch and other miscellaneous statistics. These master datasets can be found in the nba-player-archetype-main/data/MasterData folder. In addition, this package allows users to redefine the season boundaries and features in each master dataset. This can be done in the crete_datasets notebook in the nba-player-archetype-main/data/remake-master-data directory.

### b) Clutch Dataset Augmentation

As part of this project our group further augmented the clutch dataset from the features used in the Muniz paper. The clutch metric is defined as a game within 5 points with 5 minutes left. There is a leverage metric for end of game situations that was used to add a better measure of the players' ability to perform in the clutch time of games. This augmented clutch dataset can be found in the MasterData folder above. The creation of this augmented master dataset was done using the clutch_data_augmentation notebook, located in the nba-player-archetype-main/data folder, and users can run this notebook if you upon recreating the master datasets you wish to augment the Muniz clutch dataset features.

### d) CDkM Algorithm for Player Archetype Determination

The Muniz algorithm follows four main steps:

- Perform PCA on each master dataset
- Perform K-means clustering on each master dataset
- Build a weighted network
- Perform the Community Detection Algorithm on each weighted network created.

We re-created these steps via Python3, and the notebook called CSE6242_notebook can be found in the nba-player-archetype-main/clustering directory. Our group also improved the algorithm by:

- Augmenting the clutch dataset to get a higher modularity score
- Choosing the same k value for all datasets each run to prevent a combinatorial search of using multiple k values across different datasets

### e) Visualization of Results

The major feature of this package which is novel to the Muniz paper is the development of an interactive visualization. Using d3.js, an ad hoc analysis of player groups can be done. It is easy to be use, and the base version of this visualization can be run using our master datasets to allow users to explore the relationships between NBA players' skillsets in different seasons. The basis of the visualization is a force directed graph, where the player data output from the CDkM algorithm is merged with basic player statistics, and assigned as attributes to each node. When the weighted network is built it can be used to create edges between these player nodes. The result is a graph showing the communities to which the players belong, as well as the each player's relationships to the other players in the network.

To further build on the graph, we allow the user to filter the nodes by season, team, position, connected/unconnected nodes and to filter the edges by edge weight. This allows the user to understand relationships between players on teams to better understand team building and player contributions, to see how players within the same role can contribute differently to a team, or to understand which players are most closely related.

Information about each group can be found using the dropdown in the bottom left of the visualization, and individual players can be selected either by pressing enter or clicking on a node.  Once a node is selected, the user can jump to related players by pressing the right arrow key which will select another player connected to the current one based on the applied filters. 

The visualization.html file located at nba-player-archetypes-main/project-viz contains the script required to make this visualization.  In order to run it, the user must have the lib file in the same directory which contains the d3 v5 libraries required, as well as the output.csv and nodeData.csv which are read by the script to form the graph.

## 2) Installation

Steps for installation:

- Download the nba-player-archetype-main.zip file.  This can be done via the project submission or Github 
- Extract the zip file to your prefered directory

A full guide for the installation of this package and execution of the visualization can be found at: https://youtu.be/8deBX_gmqtE

## 3) Execution

There are two ways to execute the code.  The first is to run the base visualization.  This will allow you to create and interact with the NBA Player Archetypes visualization using the based node and edge csv files.  This option uses the optimal k value as derived in our report and operates over the 2014/15 to 2019/20 NBA seasons. The second method of execution is more complex, allowing for scalability by letting the user interact with the provided jupyter notebooks to recreate the visualization under their own parameters.  

### a) Method 1: Basic Visualization execution

The base visualization can be accessed solely within the project-viz folder.  To run the visualization, download the nba-player-archetype-main.zip file, and extract it to a location of your choosing.  Open the command prompt, navigate to the nba-player-archetype-main folder, and then navigate to the project-viz folder.  In order to view the visualization - a python web server needs to be run.  To run this, simply enter the following in the terminal: -m python http.server 8888 &. (python3) or -m python SimpleHTTPServer 8888 &. (python2).  Next, navigate to http://localhost:8888/ in your web browser.  This will display the base version of the visualization.  In this base version, the title is displayed in the upper left, legend in the upper right, descriptions of each group in the legend in the lower right, and dropdowns for filtering the visualization in the lower left. By default, the season is set to 2014-15, and no other filters are applied.

### b) Method 2: Changing the Visualization to fit the Users Needs

There are a few ways the user can change the visualization to perform their own experiments.  Keep in mind: re-running the algorithm can take quite a lot of time (12+ hours).

- i) Re-creating Master Dataset for Different Seasons
- ii) Augmenting Clutch Dataset
- iii) Running CDkM Algorithm
- iv) Create New Edges and Nodes for Visualization

   #### i) Re-creating Master Dataset for Different Seasons  

First, the user can change the seasons observed.  This would be done by first launching the Anaconda terminal on your device.  Navigate to the nba-player-archetype-main project folder.  Open the data folder, then the remake-master-data folder. Type in jupyter notebook to the terminal to launch jupyter notebooks, then open the create_datasets folder. In the third code cell of the notebook, the user can set the start and stop seasons for creating the master datasets.  If start were set to 2014, and end to 2022, then the datasets would contain seasons 2014-15 to 2022-23 inclusive.  Of note, the hustle data is only available for 2018 on, so the hustle statistics will only be generated from 2018 on regardless of the start year set.  Running this Jupyter Notebook will generate new master datasets with the years set by the user.  To update the visualization based on these new years, the CDkM algorithm will need to be re-run, and steps ii, iii, and iv will need to be completed.

   #### ii) Augmenting Clutch Dataset
   
After recreating the master dataset, the clutch_data_augmentation notebook will take the new masterclutch dataset and will attempt to join additional clutch data Play By Play API. A clutch_leverage could be adjusted to include just VeryHigh situations as listed in the Play By Play API, however the default includes High and Very High situations. Another parameter that could be configured is the the null threshold in the pdp clutch data preprocessing process where it drops features with over a pecentage of NULLS. The default is currently set to 0.30. Lastly, there is a list of all the feature columns that gives the user control of which feature they would like to retain. Further EDA is recommended when new features arise in the dataset. The final output will be an augumented clutch dataset writen as a new dataset in data/MasterData/Augmented_MasterClutch.csv
   
   #### iii) Running CDkM Algorithm
   
After augmenting the master dataset, the clustering notebook will take the different master datasets, and run our full CDkM algorithm for all of the years/seasons, and datasets specified in the notebook file. The adjustable parameters for the modeling are:
- seasonz/Yr: The seasons of data in the master data sets to analyze
- var2keep: variance ratio to keep during PCA (0.99 for our run)
- allDFs: a list of the names of all datasets to be used
 

   #### iv) Create New Edges and Nodes for Visualization

There could be many reasons the user may wish to regenerate the nodes and edges in the data frame.  The first is if the data has changed, for example - changing the years in the create_datasets notebook, changing the features in the master datasets or if filters are applied to reduce the players in the master datasets.  All these changes would require the CDkM algorithm to be re-run and new output would be generated.  The second would be if you wish to use a different k-value from the clustering algorithm.  

First, to re-create the nodes csv file for the visualization, navigate to nba-player-archetype-main/project-viz in the Anaconda terminal and open jupyter notebooks.  Open the createVizNodes notebook and set the values for start_season and end_season in the first cell.  These should be the same start and end seasons from your master datasets. Running all kernels will generate a file called nodeData.csv in the project-viz folder.

Next to re-create the edges csv file for the visualization, in the same directory as the previous step open the createVizEdges notebook. In this file the start_season and end_season can be set in the first cell. In addition, a value of k can be chosen.  As long as the CDkM algorithm has been run with that value of k in step iii previously be the user, that value of k can be entered in this cell.  If a value of k is chosen and the CDkM algorithm has not been run with that k value, or if start/end seasons outside the range of the master dataset are chosen, an error message will be displayed informing the user that they need to choose appropriate values. Running all the kernels in this notebook with appropriate values will generate a file called output.csv, which is read into the visualization to create the edges. 

To update the visualization after creating new nodes or edges, simply refresh the page.


