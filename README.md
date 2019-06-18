# Intro To Azure ML Visual Interface


## Create Resource in Azure
1. Go to portal.azure.com and login or create an account
2. Click create resource </br>![createres][create-resource]
3. Select "AI + Machine Learning" then "Machine Learning service workspace" </br> ![selectworkspace][select-workspace]
4. Fill in requried fields and select "Review + Create" then select "Create"
5. It will take a few minutes to create the resources needed for your workspace. Below is a list of all the resources that are created:
</br> ![resourceslist][resourceslist]

## Launch AML Visual Interface
1. Navigate to your resource group that you created the workspace under
2. Click the Machine Learning Service Workspace resource listed in the resource group
3. In the left nav click on "Visual Interface"
4. Then click "Launch visual interface"
5. This will open a new tab for the Visual interface for Azure Machine Learning Service

## First we need data
1. https://www.kaggle.com/grasslover/spotify-music-genre-list/downloads/spotify-music-genre-list.zip/6
2. Talk about kaggle here
3. Open tsv fie and save as a csv

## Review Data for process
I like to use jupyter notebook and python to review my data however you dont have to do it this way. In this tutorial I have a jupyter notebook to clean my data. In a prodution setting you want to automate the cleaning and processing of your data. In other projects I have used data access layers with C# to grab just the data I want and process out special characters and other data attributes that can cause issues downstream.

## Import data to AML Visual Interface
1. Select new
2. Select DataSets
3. Select Add
4. Select Upload from file
5. Navigate to downloaded Song data and select it to be uploaded

## Create New Expirment
1. Select New
2. Select Experiments
3. Create a blank Experiment
4. Go to My Datasets to find the song data uploaded
5. Drag data onto workspace
6. Add convert to CSV module
7. Run Expirment

## The first expirnemtn run and creating the compute resource
1. TODO Steps

## Building the model
1. Once you have the data on the workspace right click and select visualize to review your data
2. 




[create-resource]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/createresource.png "Create Resource"
[select-workspace]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/selectworkspace.PNG "Select Workspace"
[resourceslist]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/workspaceresourcelist.PNG
