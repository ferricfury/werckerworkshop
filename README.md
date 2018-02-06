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
 
 #Step 2: Clone the project Locally
 
 Pull down the content of the folder locally. If you want you can run the application locally, however, to do that you will 
 need to have Oracle Jet installed. 
 For the purpose of this workshop it is not required to do this You can find instructions on how to build an Oracle Jet application [locally here](http://www.oracle.com/webfolder/technetwork/jet/globalGetStarted.html)
 
#Step 3: Modify the Application
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

Once that is working you know have to tell the project what repository to build. To do this you must Select a Repository. You should be able to see the project we forked earlier called werckerworkshop.


