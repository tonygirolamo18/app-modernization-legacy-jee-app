
# IBM Client Developer Advocacy App Modernization Series

##  Small, legacy JEE app for multiple labs in the App Modernization Series

This is a modified version of the code for [Getting started tutorial for Liberty](https://console.bluemix.net/docs/runtimes/liberty/getting-started.html#getting-started-tutorial) for use in IBM Cloud Private developer training.

This is a minimal JEE app that requires the WebSphere Liberty Java application server and the Apache Derby  database to run.

## Required runtimes

**WebSphere Liberty Java application server:**  [WebSphere Liberty](https://developer.ibm.com/wasdev/websphere-liberty/) is a modular and easy-to-use Java application server, built on the open source [Open Liberty](https://openliberty.io/) project.

**Apache Derby:** [Apache Derby](https://db.apache.org/derby/) is an open source RDMS written entirely in Java.


## How it Works

The app consists  of a JEE web app that  prompts the user to enter their name. When a name is entered it  adds the name to a list stored in Apache Derby and then displays the entire contents of the database  

This app adheres to best practices of the [12-Factor app](https://12factor.net/). A detailed checklist can be found [here](12-factor.md).

<p align="center">
  <kbd>
    <img src="images/GettingStarted.gif" width="300" style="1px solid" alt="Gif of the sample app contains a title that says, Welcome, a prompt asking the user to enter their name, and a list of the database contents which are the names Joe, Jane, and Bob. The user enters the name, Mary and the screen refreshes to display, Hello, Mary, I've added you to the database. The database contents listed are now Mary, Joe, Jane, and Bob.">
  </kbd>
</p>

## Overview

In this lab you will  be connecting a Git repository to a Continuous Integration/Continuous Deployment pipeline built with Jenkins on IBM Cloud Private.
The app in the Git repo  is a modified version of the code for [Getting started tutorial for Liberty](https://console.bluemix.net/docs/runtimes/liberty/getting-started.html#getting-started-tutorial).

## Prerequisites

**Note:** If you've already completed [Part 1 Working with Helm](https://github.com/djccarew/app-modernization-helm-lab) you can proceed to Step 1

1. From a command line prompt issue the following commands to deploy the app

```
   # Clone the repo
   git clone https://github.com/djccarew/app-modernization-legacy-jee-app.git
   cd app-modernization-legacy-jee-app

   # Deploy the app
   # Substitute your username for [uname] (eg user04)
   helm install --name liberty-starter-[uname] chart/liberty-starter --tls
```   


###  Step 1: Set up the  CI/CD pipeline

In this section we will be connecting our cloned Git repo of [this app](https://github.com/djccarew/app-modernization-legacy-jee-app)  to set up a Continuous Integration/Continuous Deployment pipeline built with Jenkins. This pipeline contains 4 different steps as follows:

  | Stage                         | Purpose                                                                        |
  | ----------------------------- | ------------------------------------------------------------------------------ |
  | Build Application War File    | Pulls in dependencies from Maven and packages application into .war file       |
  | Build Docker Image            | Builds the Docker image based on the Dockerfile                                |
  | Push Docker Image to Registry | Uploads the Docker image to the Docker image registry within ICP             |
  | Deploy New Docker Image       | Updates the image tag in the Kubernetes deployment triggering a rolling update |

More details of this pipeline can be found in the [Jenkinsfile](./changeme).

1. Log into Jenkins using the URL provided to you by your instructor with the credentials provided to you

2. The pipeline should have already been created for you.

![Jenkins initial page](images/ss1.png)

3. Click on your pipeline to open it and then click on the **Configure** link in the navigation area at the left to change it's properties

4. Scroll down to the **Build Trigger** section and select **GitHub hook trigger for GIT SCM polling**

![Build Triggers](images/ss2.png)

5. Scroll down to the **Pipeline** section and find the **Definition** drop down menu. Select **Pipeline script from SCM** and for **SCM** select **Git**.

6. For **Repository URL** enter the url to the cloned repository that you forked earlier (i.e. https://github.com/<your username>/app-modernization-legacy-jee-app.git)

7. Ensure the **/master** branch is being targeted and click **Save**.

![pipeline config](images/ss3.png)

### Step 3: Manually trigger a build to test pipeline

1. In Jenkins in the navigation area on the left click on **Build with Parameters**. Accept the defaults of the parameters and click on **Build**

2. To see the console output click on the build number in the **Build History** and then click on **Console Output**

![Console output](images/ss4.png)

3. If the build is successful the end of the console output should look like the following:

![End of console output](images/ss5.png)

### Step 4: Trigger a build via a commit to Github

Now you'll configure Github to trigger your pipeline whenever code is committed.

1. Go back to Github and find your cloned repository (i.e https://github.com/<your username>/app-modernization-legacy-jee-app)

2. Click on the repository settings

![Settings](images/ss6.png)

3. Under **Options** select **Webhooks** and click **Add webhook**

![Add webhook](images/ss7.png)

4. For the Payload URL use <Jenkins URL>/github-webhook/  where <Jenkins URL> is the  URL you used  to login to Jenkins (**Note** Don't forget the trailing /)

5. Change content type to **application/json**

6. Accept the other defaults and click **Add webhook**

![Add webhook](images/ss8.png)

7. In the Github file browser drill down to src/main/webapp/index.html

8. Click on the pencil icon to edit index.html  and on line 2 locate the **\<html lang="en">** tag.

10. Change the **"en"** to one of the following:

- es: Spanish
- pt: Portuguese
- fr: French
- ja: Japanese

This will change the language that loads on the webpage to whatever language you selected.

11. At the bottom of the UI window add a commit message and click on **Commit changes**

12. Switch back to Jenkins  and open the pipeline that you were working on  earlier.

13. Verify that your pipeline  starts building.

14. When the pipeline is finish deploying, launch the IBM Cloud Private Web UI using the URL given to you by your instructor and login in.

15. In the Navigation area on the left expand **Workloads** and select **Helm Releases**

16. Look for your Helm release in the list and click on the **Launch** link on the right.

17. Verify that the app's UI opens in another tab and the language matches the one you selected.

## Summary
You created a Jenkins pipeline to automatically build and deploy an app that has been updated in Github .
