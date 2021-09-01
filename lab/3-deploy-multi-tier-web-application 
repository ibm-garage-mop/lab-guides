# Introduction

It is a more advanced example with the deployment of a NodeJS web application connected to a MongoDB backend.
You can get an overview of the app in the Github project: https://github.com/geoffrey-pascal/lets-chat

#	Deploy a multi-tier Application composed of NodeJS application and a MongoDB backend

## Create a new project
First, go back to the Create Project page by clicking Developer, select Project: sushiweb

* Click Create Project

Enter Name `letschat` and  click `Create`

## Deploy MongoDB

Click `+Add` and `From Catalog`

Filter the output with `mongodb` string.

Select the service `MongoDB (Ephemeral)`:

Click `Instantiate Template`

Enter the following informations and click “Create”
*	Namespace: openshift
*	Database Service Name : mongodb
*	MongoDB Connection Username: mongouser
*	MongoDB Connection Password : password
*	MongoDB Database Name : sampledb
*	MongoDB Admin Password: mongoadm
*	Version of MongoDB Image : 3.6

Check the status of the Pod, click Developer / Topology, click DeploymentConfig `mongodb`

You deployed a new MongoDB instance into your project. This instance as it’s suggested by the name is ephemeral meaning that the data are not persistent when you restart the instance. This service should only be used for development purpose.

Click on the Pod `mongodb-1-xxxx` to have details, check that it is in Running state

##	Deploy NodeJS application

* Click “+Add”

Select “From Git”

Enter `https://github.com/geoffrey-pascal/lets-chat` for the `Git Repo URL`

Openshift automatically detects that it is a NodeJS application.

Scroll down, enter `letschat` for the Name and click `Routing`:

Set the hostname to letschat.10.0.190.99.nip.io and Target Port to 8080 and click “Create”.

Click on `letschat` to check the Deployment status.

When the application deployment is completed `Build 1 is complete  , try to connect to the application by clicking on the Open URL
letschat.10.0.190.99.nip.io

First, create an account when the login screen is displayed.

Click “I need an account”

Set dummies values for the account, remember the username and the password you will need to login into the application and click “Register”

Try to login with your username and password.

Then create a chat room by clicking on the `+` icon and filling the form

Now you are able to use the application. You can see that your NodeJS chat application is successfully connected to your MongoDB instance.
