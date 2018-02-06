# Wercker Workshop
In this workshop we are going to Build an Oracle Jet Application, Push the application to GitHub and then Build the Project with Wercker.

The Wercker application will build the Oracle Jet application, 
push the built container to DockerHub and finally send a command to Kubernetes to deploy the application.

# Prerequisite

To build the application you will need to create a few accounts if you do not already have them.

 1. GitHub Account - This will be where we store our source code. If you do not have one or can't create one, speak to me. I have a spare one you can use.
 2. DockerHub Account - This will be where we store our containers
 3. Wercker Account - You can use your GitHub credentials to create an account here.
 
 # Step 1: Fork the Project
 
 Fork this project to create your own verion of this project in your GitHub Account. 
 Press the Fork button which on my screen is at the top right of the screen.
 
 # Step 2: Clone the project Locally
 
 Pull down the content of the folder locally. If you want you can run the application locally, however, to do that you will 
 need to have Oracle Jet installed. 
 For the purpose of this workshop it is not required to do this You can find instructions on how to build an Oracle Jet application [locally here](http://www.oracle.com/webfolder/technetwork/jet/globalGetStarted.html)
 
# Step 3: Modify the Application
First lets change some text in the application. Go to the Index.html page and change 
Hello Dubai to Hello "Your Name". So for me I would change it to Hello Martin.

# Step 4: Check the application in

Open up a command prompt or terminal prompt in the folder of your project and check your application into GitHub using

``` 
git add .
git commit -"Making change to index.html"
git push 
```

# Step 5: Set up a Wercker Application

To create an application in Wercker log in a the website and click "Create your first application" it will ask you to associate your GitHub account.

Once that is working you know have to tell the project what repository to build. To do this you must Select a Repository. You should be able to see the project we forked earlier called werckerworkshop. Select it and click next. You will than be asked to add an SSH key to your GitHUb account this will give Wercker access to your project and add a webhook so that the build will be triggered when you check the project in. Click next. On the review page check everything looks good and then click next.

# Step 6: The Wercker File

The next screen asks you to add a wercker.yaml file to the base of the project. In the drop down menu select node.js as the project language.

It will then create a sample wercker.yml file for you. In your project add a file called wercker.yml and copy and paste the contents of the sample wercker file into the this file.

# Step 7: Check in your change

Check your changes into Github, this should now kick off the build. In a terminal or command prompt inside the project folder type

``` 
git add .
git commit -"Adding Wercker file"
git push 
```

If you go to you application on the Werker website you should now see that the project has built. But Lets make it a real build. 

# Step 8: OJet Build

Delete everything that is in the Wercker file and replace it with the following:

```
box: node
build:
  steps:
    - npm-install
    - npm-test
    - script:
        name: oracle jet build
        code: |
          npm install -g @oracle/ojet-cli
          ojet build
    - script:
        name: copy code to output
        code: cp -r web "$WERCKER_OUTPUT_DIR"
    - script:
        name: copy yml file to output
        code: cp service.yml "$WERCKER_OUTPUT_DIR"
```

This code creates the steps required to build an Oracle Jet Project. You can see that we call ```npm install -g @oracle/ojet-cli``` to install the oracle Jet build tool and then call ```ojet build``` to run the build tool against our sorce code.

The then take the Web folder that is created by this build and copy it to the ```$WERCKER_OUTPUT_DIR``` this is a system varaible basically points to a folder location.

# Step 9: Push a container to Docker Hub

In the wercker file now add the following code:

```
push-release:
  box:
    id: nginx:alpine
    cmd: /bin/sh
  steps:
    - script:
      name: mv static files
      code: |
        rm -rf /usr/share/nginx/html/*
        mv web/* /usr/share/nginx/html
    - internal/docker-push:
        disable-sync: true
        repository: $DOCKER_REPOSITORY
        username: $DOCKER_USERNAME
        password: $DOCKER_PASSWORD
        registry: https://registry.hub.docker.com/v2
        tag: $WERCKER_GIT_COMMIT
        cmd: nginx -g 'daemon off;'
```
This code overide the default node box and creates an basic nginx box. This is just a basic webserver container. We no longer need node as the Oracle Jet application is simply some HTML, CSS and JS now that it is built. 

You will notice that there are some varible in that file

```
        repository: $DOCKER_REPOSITORY
        username: $DOCKER_USERNAME
        password: $DOCKER_PASSWORD
```

We will need to add these to the system before the build will work. First, however, we need to create the repository on Docker Hub.

# Step 10: Create a repository in Docker Hub

Login to docker hub and click the "Create Repository" button. Give the repository the name "werckerworkshop" and for now just leave it as public. You can ignore the description fields. Just click to create.

I know have my repository name which is: ```thebeebs/werckerworkshop```

# Step 11: Create the Enviromental variables

Now we have a repository we can go about setting up our enviroment names. Open wercker and look for a tab called "Environment" here add the following 3 keys:

Key | Value 
--- | --- 
DOCKER_REPOSITORY | yourDockerAccount/werckerworkshop
DOCKER_USERNAME | YourUsername
DOCKER_PASSWORD | YourPassword












