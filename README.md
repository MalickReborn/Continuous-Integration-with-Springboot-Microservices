# Springboot-Microservice
Springboot-Microservice

The objective of this repository is to show an example of continuous integration pipeline for a java application using the stack git-jenkins-docker-maven-sonarqube.
the process is to get jenkins as our Continouous integration tool to be able to pipe code from git and build and make integration with other tools for static code analysis and build docker image and push to a container image registry.

the prerequisites for this project is:

- a container image hub account / for example dockerhub
- a vm provisioned / you can pick a cloud vm like ec2. I made it on my local virtualbox
- a scm hub account / we picked github

We will then install on the provisioned machine:
- [docker](https://docs.docker.com/engine/install/)
- [jenkins](https://www.jenkins.io/doc/book/installing/)
- [maven](https://maven.apache.org/install.html)
- [sonarqube](https://docs.sonarsource.com/sonarqube/latest/setup-and-upgrade/install-the-server/introduction/)

for further details about installations look [here](https://github.com/iam-veeramalla/Jenkins-Zero-To-Hero/blob/main/README.md)

We can enter ito the project now.

I. Prepare Jenkins for the task

As a Continuous integration tool jenkins interact with the other tools using plugins and a system of credentials.
So since we will make integration with github, dockerhub and sonarrqube we have to fulfil it here too.

For the plugin part we have the path Manage jenkins/plugins/available plugins to look for docker pipeline , SonarQube Scanner for Jenkins, Maven Integration plugin
then go to Manage jenkins/plugins to configure maven as ahowed below and restart jenkins.
[![Screenshot-from-2023-12-06-11-36-59.png](https://i.postimg.cc/L6X7Rvp7/Screenshot-from-2023-12-06-11-36-59.png)](https://postimg.cc/Z0Gjx80F)

We then go to our Sonarqube dashbord to the right side to click on the 'A' in the top corner then 'My account/security/generateTokens' 
We had a name , generate token and go back to jenkins to create a credentials to the path Manage_jenkins/credentials/system/Global credentials (unrestricted)/add credentials/ and pick secret text and put the patch the generated token to create a credential
the same way to the path Manage_jenkins/credentials/system/Global credentials (unrestricted)/add credentials/ we chose username and password to put our dockerhub credentials and record it .
the result has to look something like that:
[![Screenshot-from-2023-12-06-11-32-33.png](https://i.postimg.cc/wxWfWJmk/Screenshot-from-2023-12-06-11-32-33.png)](https://postimg.cc/w3mQM1Ft)
We then restart Jenkins once again to make it integrate the new changes.

we can then build our project since the configuration are all done.

 We go back to jenkins dashboard , select item , name our project and pick pipeline ok 
[![Screenshot-from-2023-12-06-09-05-06.png](https://i.postimg.cc/PxbxXTmt/Screenshot-from-2023-12-06-09-05-06.png)](https://postimg.cc/hzjgMFyw)

 our project looks like this
[![Capture-d-cran-du-2023-12-13-00-05-07.png](https://i.postimg.cc/T3DXvGMh/Capture-d-cran-du-2023-12-13-00-05-07.png)](https://postimg.cc/JsMFjfhW)
[![Capture-d-cran-du-2023-12-13-00-06-10.png](https://i.postimg.cc/KjHCCp0n/Capture-d-cran-du-2023-12-13-00-06-10.png)](https://postimg.cc/BP2hKp96)
[![Capture-d-cran-du-2023-12-13-00-07-04.png](https://i.postimg.cc/fbN149fg/Capture-d-cran-du-2023-12-13-00-07-04.png)](https://postimg.cc/sMTT718p)

the better choice with regards with the best practices  for the pipeline definition is SCM.
The path of our [Jenkinsfile](https://github.com/MalickReborn/Continuous-Integration-with-Springboot-Microservices/blob/main/user-service/Jenkinsfile) 

all this done we can launch the build process 
[![Capture-d-cran-du-2023-12-13-00-14-35.png](https://i.postimg.cc/SxNDcfV4/Capture-d-cran-du-2023-12-13-00-14-35.png)](https://postimg.cc/MvN7SRh3)

we can see that our pipeline has successfully analysed our static code and pushed buld and push our image on docker registry
[![Capture-d-cran-du-2023-12-13-00-23-20.png](https://i.postimg.cc/pLRXCpnR/Capture-d-cran-du-2023-12-13-00-23-20.png)](https://postimg.cc/BX70QZgV)
[![Capture-d-cran-du-2023-12-13-00-24-19.png](https://i.postimg.cc/6pMwhzXf/Capture-d-cran-du-2023-12-13-00-24-19.png)](https://postimg.cc/VJ03fq5J)

in order for the pipeline to be triggered every time we have a change in the source code on github we must configure a web hook on github

Switch to your GitHub account.
Now, go to the “Settings” option on the right corner, as shown in the image below.
Jenkins GitHub Webhook - Setting Button in GitHub

Here, select the “Webhooks” option and then click on the “Add Webhook” button, as shown in the image below.
Jenkins GitHub Webhook - Adding Bew Webhook in GitHub

It will provide you the blank fields to add the Payload URL where you will paste your Jenkins address, Content type, and other configuration.
Go to your Jenkins tab and copy the URL then paste it in the text field named “Payload URL“, as shown in the image below.


Append the “/github-webhook/” at the end of the URL.
The final URL will look like in this format “http://address:port/github-webhook/“.


The “Secret” field is optional. Let’s leave it blank for this Jenkins GitHub Webhook.
Next, choose one option under “Which events would you like to trigger this webhook?“. The 3 options will do the following events listed below:
Just the Push Event: It will only send data when someone push into the repository.
Send Me Everything: It will trigger, if there is any pull or push the event into the repository.
Let Me Select Individual Events: You can configure for what events you want your data.
Now, click on the “Add Webhook” button to save Jenkins GitHub Webhook configurations.
That’s it! You completed Jenkins GitHub Webhook. Now for any commit in the GitHub repository, Jenkins will trigger the event specified.




  
  

