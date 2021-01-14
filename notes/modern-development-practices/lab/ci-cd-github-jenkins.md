# Integration of GitHub with Jenkins

### Instructions

* One of the developer in a team will create *new branch* to add `Jenkinsfile`. 
* The developer shall create a pull request and add other developer/s as reviewers. 
* Other developer will review the pull request as well approve and merge to master.
* Each developer will create seperate branch to write atleast one test case for the feature they worked on in Lab Test - 4.
* Each developer shall create a pull request and add other developer/s as reviewers. 
* Other developer will review the pull request as well approve and merge to master.
* Start jenkins server and follow from **step 4** onwards using [Install jenkins](../../overview-of-devops/lab/install-jenkins.md).

### One of the developer can follow below steps to integrate jenkins with GitHub

* Goto Jenkins UI and **Open Blue Oceans**

![jenkins-github-01](./jenkins-github-01.png)

* Let us create a new pipeline

![jenkins-github-02](./jenkins-github-02.png)

* Select the remote version control system from available options

![jenkins-github-03](./jenkins-github-03.png)

* Connect Jenkins to the remote VCS via *personal access token*

![jenkins-github-04](./jenkins-github-04.png)

* Owner of the repository can go to [https://github.com/settings/profile](https://github.com/settings/profile)

![jenkins-github-05](./jenkins-github-05.png)

* Click on personal access token

![jenkins-github-06](./jenkins-github-06.png)

* Click **Generate new token**

![jenkins-github-07](./jenkins-github-07.png)

* Make the scope selection as shown.

![jenkins-github-08](./jenkins-github-08.png)

* Copy personal access token 

![jenkins-github-09](./jenkins-github-09.png)

* Paste the personal access from clipboard

![jenkins-github-10](./jenkins-github-10.png)

* Select your **username / organisation**.

![jenkins-github-11](./jenkins-github-11.png)

* Click on create pipeline.

![jenkins-github-12](./jenkins-github-12.png)


This is the sample of the screenshot after build is triggered via GitHub repository

![jenkins-github-13](./jenkins-github-13.png)

Make the submission on BlackBoard.