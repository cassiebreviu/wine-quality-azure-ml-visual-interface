# Intro To Azure Machine Learning Visual Interface (VI)


## Create Resource in Azure
1. Go to [Azure Portal](https://azure.portal.com/) and login or [Create an Account](https://azure.microsoft.com/en-us/free/)
2. Click "Create resource" </br>![createres][create-resource]
3. Select "AI + Machine Learning" then "Machine Learning service workspace" </br> ![selectworkspace][select-workspace]
4. Fill in requried fields and select "Review + Create" then select "Create"
5. It will take a few minutes to create the resources needed for your workspace. Below is a list of all the resources that are created:
</br> ![workspaceresourcelist][workspaceresourcelist]

## Launch AML VI
1. Navigate to your resource group that you created the workspace under
2. Click the "Machine Learning Service Workspace" resource listed in the resource group
3. In the left nav click on "Visual Interface"
</br> ![navvisualinterface][navvisualinterface]
4. Then click "Launch visual interface"
</br> ![launchvisualinterface][launchvisualinterface]
5. This will open a new tab for the Visual interface for Azure Machine Learning Service
</br> ![viportal][viportal]

## We need data!
1. I used a dataset I found on Kaggle. Kaggle is an online community of data scientists and machine learners. 
2. Download the dataset from this repo because I have added an additional field (quality bool) to the dataset.
* [Wine Dataset from Repo](https://github.com/cassieview/IntroToAzureMLInterface/blob/master/dataset/winequality-red.csv)
* [Kaggle Dataset](https://www.kaggle.com/uciml/red-wine-quality-cortez-et-al-2009) 
</br> _Relevant publication: P. Cortez, A. Cerdeira, F. Almeida, T. Matos and J. Reis. Modeling wine preferences by data mining from physicochemical properties. In Decision Support Systems, Elsevier, 47(4):547-553, 2009._

## Getting data into AML VI
There are a few different ways to import data into VI. You can use the [Import Data Module](https://docs.microsoft.com/en-us/azure/machine-learning/algorithm-module-reference/import-data) to import data from Azure Blob Storage or a Web URL via HTTP. In this tutorial we are going to upload our data into the "My Datasets" in AML VI.
1. Select "New" from the bottom left corner of the browser
2. From the left nav bar Select "Datasets"
3. Select "Upload from Local file"
</br>![newdatasetupload][newdatasetupload]
4. Navigate to downloaded data and select it to be uploaded
5. Update the name and add a description (its helpful to have detailed description once there are lots of datasets uploaded)
</br>![datasetuploadfields][datasetuploadfields]

## Create New Experiment
1. Select "New" from the bottom left corner of the browser
2. Select "Blank Experiment"
3. In the top left hand of the workspace select the experiment name text "Expirment created on xx/xx/xxxx" and edit the name of your expirnment.
4. Go to My Datasets to find the data uploaded OR use the import module to import from the github csv link
5. Drag data onto workspace
</br> ![createexpadddata][createexpadddata]

## Assign Label column and Visualize data
We now have created an experiment and have imported the data. We are going to use all the data in the spreadsheet   
1. Assign the Label data column by adding a "Edit Metadata" module
2. 

## The First Run of the Experiment
1. Select "Run" from the button of the workspace
2. Select "Create new" to create a new compute target
3. Enter a name for the new compute target
4. Select "Run"
</br>![runexperiment][runexperiment]


# Deploy the Web Service
1.

## Things to look out for
## we have 100% accuracy which doesnt mean its awesome.. it could just mean its overfitted
1. Overfitting a model means you dont have enough data for it to actual "learn" so it will do great on your data but as soon as it put out into the real world. It will fail. This is why you want to always test with unseen data. Lets grab some data from another source to see if we actually have a realy good model or if its overfitted.
2. If data is very imbalanced meaning you have lots of one label and little of another - this can also create overfitting and models that look like they are performing really well when they are actually not good models. There are different techniques to work with imbalanced data.
* sampling
* SMOTE
* WHATELSE??

## Terms

Feature:
Label:



[create-resource]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/createresource.png "Create Resource"
[select-workspace]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/selectworkspace.PNG "Select Workspace"
[workspaceresourcelist]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/workspaceresourcelist.PNG

[viportal]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/viportal.PNG
[navvisualinterface]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/navvisualinterface.PNG
[launchvisualinterface]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/launchvisualinterface.PNG
[runexperiment]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/runexperiment.PNG
[datasetuploadfields]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/datasetuploadfields.PNG
[newdatasetupload]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/newdatasetupload.png
[createexpadddata]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/createexpadddata.gif
