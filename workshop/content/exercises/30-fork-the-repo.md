Is this exercise you will fetch the source code that will be used for the rest of the exercises.  

You will make a copy (fork) of the source code repository in GitHub and then build and deploy it to OpenShift.   The application we will use is a simple voting application which collects answers to a simple question from users and displays the collected results. 

## Fork the repository 

First, set up a source code repository in your own GitHub account.  You will need your GitHub username and password for
the next step.  If you don't have an account, first sign up for one at [http://github.com/join](https://github.com/join) and then come back and follow the instructions here. Be sure to remember the username and password!

 - ``Note: Github is a site based on an open source version control system known as git, like other version control software git helps with tracking changes to a repository of files.`` 

First, ensure you are in the top level home directory:

```execute 
cd ~/ 
```

The next step will fork the GitHub repository ``flask-vote-app`` into your GitHub account.  The flask-vote-app repository contains a voting application based on the Python web application framework called Flask. 

Run the next helper script and enter your GitHub username and password when prompted:

```execute 
fork-repo sjbylo/flask-vote-app
```

 - ``Note``: This command will also generate a GitHub personal access token - scoped only to the forked repository - which will be used in the further exercises. It will be removed from the workshop environment in the final clean-up exercise.  

You should now have your own GitHub repository containing the example source code. 

```execute
ls -ld flask-vote-app 
```

Change into the new repository directory:

```execute
cd flask-vote-app
```

Run this command to verify the new repository belongs to you.  

```execute
git remote -v
```

You should see your GitHub username in the output. For example:

```
...github.com/johnsmith/...
```

---
That's the end of this exercise.

Now you have forked the source code, the next exercise will get that code running on OpenShift. 


