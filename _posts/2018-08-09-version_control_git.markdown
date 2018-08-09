---
layout: post
title:      "Version Control/Github"
date:       2018-08-09 15:48:12 -0400
permalink:  version_control_git
---


I never realized the importance of Github untill recently when I made some changes to one of my biggest project's and didn't realize  it broke a lot of other stuff.  I kept coding untill one day I tried to do a simple signup action and found  nothing was working. I was so lost, without even knowing what caused it, I kept debugging, until I finally realized it was no use. Github was a rescue hero! It allowed me to go back to my Git commits and I pulled the one which was working a couple of days ago and started working on my project again. 

What a great feature to be able to go back in time..

Lets first discuss what Git is? 

A Google search yields  [Git Meaning](https://www.google.com/search?ei=TkgUW8eBCpOO8APuqqOQAQ&q=git+meaning&oq=git+&gs_l=psy-ab.1.1.35i39k1l2j0i67k1l8.6093.6093.0.8191.1.1.0.0.0.0.119.119.0j1.1.0....0...1.1.64.psy-ab..0.1.118....0.ByDMVvhIFwA)
Whaaat?   Anyways, it seems git is a derivative of the word "get'. The actual definition of Github is that GitHub not only brings together the world's largest community of developers to discover, share, and build better software from open source projects to private teams but it also plays a great role in Version Control.

## Version Control
Version Control is the process of storing multiple versions of a single project, allowing each version to be recalled at a later date. VCS (Version Control Software) are sometimes known as SCM (Source Code Management) tools or RCS (Revision Control System). One of the most popular VCS tools in use today is called Git. Git uses a database to store all the changes made to a project referenced by a label (commit) and can be used despite a stack of new commits on top of it. 
Lets see how to use a Git

### **Connecting Your Project To Github**
First, sign up for a git account. After signing up you can create a new repository by clicking "Start a project"
specify a name for this project (in this case, "TestingGit" and you can leave it public or can make it private. Next go to your local project root directory (or if you dont have one, create one in the terminal) and follow the steps below
 

```
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:anniezh12/testingGit.git
git push -u origin master
```

`git init` will initialize a new Git Repository in your local project

`git add` will add any new files to your project that you want git to keep track of. Most commonly, `git add .` is used which will actually add all the changes made to your project.

`git commit -m "first commit"` Git will not know about changes that have been made to your project.
To inform git about these changes we use ` git commit` with a message. Once you've made a commit, you will essentially have made a copy of your code at that moment, which can be accessed at any time in the future.

`git remote add origin git@github.com:anniezh12/testingGit.git`  This step will actually make a connection from the local repository to a remote one

`git push -u origin master` So appearently we have made a commit and git knows about it but if we go back to see our repository on github we won't be able to see a copy of our code labeled as `first commit`. Beacause we haven' t
pushed our final code to remote repository yet... which can be done by `git push -u origin mster`.  `git push`  takes two arguments, The first is the name of the remote repo(Where origin is an alias to our remote repo testinGit. We can also change it to anything using following command in the terminal
  `git remote rename origin testingGit`) 
	and the second is the name of the remote branch you want to send code to.

**Note** The above steps are to be taken first time after that every time you make any changes that you want to save as a commit follow following  three steps

1. git add  .
2. git commit -m " Any meaningful detailed message about the changes"
3. git push origin master

### **Git Pull**
Above I am giving an example of  a simple  project  that I made and connected to Github which is not the case in most situations. Often more than one developer adds code to a project and in that case you need to pull the fresh code to your local repository before pushing new changes or even before making any changes to your code which can be done by 
`git pull`.

### GIT Fork and Clone

 Moreover, you  can use/copy any public repository in your local environment by simply navigating to the desired repository and follow the following steps
 
  1. Press Fork button on the repository page, A fork is a copy of a repository. Forking a repository allows you to freely           experiment with changes without affecting the original project.
  2.  Once Forked you can either clone or download  the repo, I usually clone it by clicking the clone button and then               copying the provided link.
  3.  Simply paste the copied link with this git command  `git clone  copied-link` in your terminal
  4.  Now you have a copy of all the code you can modify and can use the way you want!
  
### 	Changing the remote repository

   To check which remote repository is attached to our local project simply type 
	  ` git remote -v`   //This will show us the current url of the remote repository 
		
		In order to change the remote repository add the following command in the terminal
				`git remote set-url origin https://github.com/user/repo-to-be-added-as-remote.git`
		 
		 

### **Time Machine**

Ok so now how to get back in time. Github has made it very simple as well. In my case I simply need to delete the unwanted copies of code which I accomplish by ` git reset --hard commit_number`
where commit_number is the commit you want to make your current state of the code and can be found by 
`git log` command.
Finally `git check -b branch_name` now you are on the the branch you want to work on.

