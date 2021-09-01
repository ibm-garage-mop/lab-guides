# Introduction

Let’s say you want to make your own Sushiweb website. You need to rebuild your own container from the application sources with your modifications. Luckily, Openshift provides several features to help you in the build process and delivery of the application to the end users

In the following exercise you will pull the Sushiweb source code from Github, then build and deploy the container image in your Openshift cluster. Once the website is deployed you will pull a new development version from Github and live update your running
pods.

The Sushiweb website sources are hosted on Github, you can check the directory structure of the project with this URL: https://github.com/geoffrey-pascal/sushiweb

As you can see, this project contains a Dockerfile file describing how to build the image of the application. Openshift can automatically run this file, build a container image and push it to its private registry.

#	Build a website container from Dockerfile

## Create a new project
First, go back to the Create Project page by clicking Developer, select Project: `sushiweb`

*	Click Create Project

Enter Name “sushiweb-sources” and  click “Create”

## Create an OpenShift deployment from Github source code

*	Click `+Add` and `From Dockerfile`

Enter `https://github.com/geoffrey-pascal/sushiweb` for the Git Repo URL.

Scroll down, enter sushiweb-sources for the Name and click `Routing`:

Set the hostname to `sushiweb-sources.10.0.190.99.nip.io` and Click  `Create`.

* Click on `sushiweb-sources` to check the Deployment status

When the pod is in Running state, click the link to the route, check your website availability:

You will be connected to the deployed web site.

## Scale the application

1.	In the OpenShift web console, click `Developer / Topology` in the left Pane.

* Click on `sushiweb-sources` to display the Deployment, select the Details tab.

Now Scale your application from 1 pod to 3 pods

When all pods are ready observe the following icon:

2.	Switch to the “Resources” tab:

You should now see 3 pods deployed and can access to the logs of each pod:

You can see your 3 pods are serving requests in parallel.

#	Experiment Rolling updates

Let’s say a developer has created a new version of Sushiweb v1.0 and your job is to deploy this updated version.

Check GitHub to discover the new branch: https://github.com/geoffrey-pascal/sushiweb/branches

* Click on `Developer / Builds` and select the `sushiweb-sources` build.

* Click “Edit Build Config” in the upper right corner:

The git field contains the URI to the remote Git repository of the source code, to check out a specific Git add a line `ref: v1.0` after the `uri: 'https://github.com/geoffrey-pascal/sushiweb'`, This branch contains the new development version of Sushiweb, click “Save”

* Click “Start build” to start the build process again:

In the OpenShift web console, click “Developer / Topology” in the left Pane

Click on `sushiweb-sources` to display the sushiweb-sources Deployment, select the Resources tab.

There are 2 Build in complete status, click on View logs to see the log stream.

Check the new website deployed. You may need to refresh your browser cache to see the updated content (The headings of the website should be displayed in blue).
