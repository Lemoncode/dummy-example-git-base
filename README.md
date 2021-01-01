# Learn to use Git Terminal by example

If you are used to work with GUI tool and want to learn the basics of working using the terminal, keep on reading.

In this readme you will find a step by step guided tutorial that will let you learn how to manage with terminal by doing.

In this repo you have a very simple project (a web project that displays a number via the console log), we will provide a guide to:

- Recreate the repo from scratch.
- Commit and push changes.
- Create branches.
- Merge branches.
- Resolve conflicts

We will use git terminal commands to setup and work with the repo.

Prerequisites to go through this tutorial:

- Install [Git](https://git-scm.com/downloads).
- Install [nodejs](https://nodejs.org/es/).

# User settings
It is important before starting to establish your username and email address, because this information will be appear in the "commits". 
1. Set up your username in Git
```bash
$ git config --global user.name "Test name"
```
2. Set up your email address
```bash
$ git config --global user.email test@example.com
```

If you want to check your configuration, you can use the command:
```bash
$ git config --list
```

# Creating the repo

You can choose whether to use HTTPS (your standard git login user) or SSH (a more secure approach to treat with your repos but need some additional configuration).

> More info about how to [create an SSH key on your computer](https://confluence.atlassian.com/bitbucketserver/creating-ssh-keys-776639788.html), and [set it up on Git](https://help.github.com/en/enterprise/2.18/user/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account)

To create the repo you can just click on the _new repository_ button in Github or your favourite repository cloud provider.

> When you create a new project, is a good idea to check the option to create a default _readme.md_ file "creating" a default readme.md, by doing that we will get an initial commit done.

# Cloning the repo

Now that you have your repo created is time to bring it to your local computer, in order to do that we clone it (download the repo to a local computer folder).

> Ensure you have permissions to access that repositories, and if you are using SSH you have it properly configured (generated locally your ssh private and public keys, uploaded to your repository provider your public key).

Choose a folder to download the files, open a terminal window, and execute the following command:

```bash
git clone <repository_address>
```

> Usually your git repo provider display an input where you can copy the url / ssh address of the repo to be cloned.

![Clone with SSH or HTTPS](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/clone-with-ssh-or-https.png)

# Adding a very simple web project

**This is not related with Git, we are just creating a very simple web project to play with**

Let's add some content to the repository.

We will start by creating a very simple web project (this is not related with Git, just setting up a nodejs project and a simple web project that displays a sentence via console log), let's execute the following command:

```bash
npm init -y
```

and install a bundler:

```
npm install parcel rimraf --save-dev
```

Let's create the following files under _src_ folder:

_./src/index.js_

```javascript
const sampleNumber = 1;
console.log(`Hello number ${sampleNumber}`);
```

Let's create a HTML entry point:

_./src/index.html_

```html
<html>
  <body>
    <h1>Check the console log</h1>
    <script src="./index.js"></script>
  </body>
</html>
```

Let's add the following command to the existing _package.json_ file:

_./package.json_

```diff
"scripts": {
+    "start": "rimraf dist && parcel ./src/index.html --open",
    "test": "echo \"Error: no test specified\" && exit 1"
},
```

Time to give a quick check that the project is working, from the terminal execute:

```bash
npm start
```

A web browser will be opened and if you open the console log from that browser (e.g. on windows pressing F12) you can check that a message is displayed.

So far so good... now that we have a very simple project to play with, let's start interacting with Git!

# Making our first commit

Maybe we think we are now ready to make our first commit to master (the default branch has been created in our project).

Before making a commit let's check which files has been modified, open the terminal and type the following command:

```bash
git status
```

![Initial Git status](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/initial-git-status.PNG)

Wooooot !!! We have only created some files, Why we get a big ton of files in our list? Well _npm install_ did it, you get under the _node_modules_ a ton of files that you don't need in you repository (and you must not upload them), we have to ignore them, let's create a _.gitignore_ file and include there the following content:

_./.gitignore_

```
/node_modules
/dist
/.cache
```

By doing that we are telling to ignore any path that includes _node_modules_ to ignore it (do not taking into account in Git), [click here to get more info about how to enter entries and patterns in a .gitIgnore](https://git-scm.com/docs/gitignore).

Now if we execute again a _status_ command we will see that only the files that we have created are pending to be added:

```bash
git status
```

![Git status after gitignore added](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/after-gitignore-git-status.PNG)

Now if we want to push this changes to our git server, we need to perform three steps:

- First we need to mark that files as ready to be committed (you can do it file by file or you can select all files).
- Second you need to commit that staged files to your local git database
- Third now you can push these changes to the server.

Let's move the files to staging:

```bash
git add .
```

Now we can commit the files to our local database, we will add _-m_ command to add a commit message:

```bash
git commit -m "initial project setup"
```

![Git commit](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-commit.PNG)

We are ready to upload the local changes that we have committed to the server:

```bash
git push
```

To check that everything went well, open your web browser, navigate to your git provider web client and check that the uploaded content is available.

# Creating and merging branches

Now that we now have taken our first steps using git is time to move forward, we are going to learn the basics of creating branches.

A branch allow us to create an isolated copy of the master branch (or any other branch) and let us work in that code cut isolated from the rest of the team, this is really useful to:

- Avoid adding not completed features to our product.
- Avoid impacting other developers in the team by adding changes into their code base.
- Reviewing the work done before merging that code into the master branch.

We want to implement a new functionality, display in the console log a second number. To avoid impacting other developers meanwhile we are progressing in our case we are going to create a branch. Let's create a new one:

```
git branch feature/display-number-b
```

> About the naming convention used to define the branch name, we are using gitflow, [click here to get more info about how to name branchs and gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

We can execute the command _git branch_ to check that the branch has been created, this command will indicate us as well the active branch (master):

```bash
git branch
```

Let's switch from our _master_ branch to the _feature/display-number-b_

```bash
git checkout feature/display-number-b
```

![Git checkout branch](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-checkout-branch.png)

> You can switch from a given branch to another if the active branch has no changed pending to be committed (if the active branch has changes pending to be committed, you have to commit them, discard them or stash them, we will check all this later on).

Another faster way to create a branch and switch to it is as follows:

```bash
git checkout -b feature/display-number-b
```

![Git checkout -b](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-checkout-b.PNG)

Now we can start working on our branch without impacting users that are working on the master branch.

Let's add some updates in the _.src/index.js_ files:

_./src/index.js_

```diff
const sampleNumber = 1;
+ const sampleNumberB = 2;
- console.log(`Hello number ${sampleNumber}`);
+ console.log(`Hello number ${sampleNumber} {sampleNumberB}`);
```

Save the changes, and add them to staging:

```bash
git add .
```

> This command adds to the index any new file or modified file.

Commit them to our local database:

```bash
git commit -m "adding one more number"
```

![Git commit](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-commit-first-change.PNG)

And push them to our remote origin (in this case the branch new created branch does not existing in the server, we need to add some extra parameters to indicate the origin to create the branch).

```bash
git push -u origin feature/display-number-b
```

Now we could merge this remote branch to master, but we won't to do this right now: we will first simulate that in between that time another developer has created another branch and modified the _./src/index.js_ file creating a conflict, we will learn how to sort this out in the following section.

> Some good advice: never merge from a given branch to master using the command line, rather raise pull requests and review the code (you can raise pull request from a given branch to master using the web client your favourite repository), on the other hand is a good practice to merge master into your branch using the terminal, this will let update your codebase to the current master status.

# Handling conflicts

Before merging our new branch to master, let's simulate that in the mean time another developer (or ourselves) has created a second branch and it's modifying the files that we have just updated.

Let's hop in the master branch:

```bash
git checkout master
```

Let's create a new branch from master:

```bash
git branch feature/display-number-c
```

Let's jump into that branch

```bash
git checkout feature/display-number-c
```

Let's update the _index.js_ file and include a _numberC_ to be displayed

_/.src/index.js_

```diff
const sampleNumber = 1;
+ const sampleNumberC = 3;
- console.log(`Hello number ${sampleNumber}`);
+ console.log(`Hello number ${sampleNumber} ${sampleNumberC}`);
```

Let add stage the updated file:

```bash
git add .
```

Let's commit this file

```bash
git commit -m "Adding third number"
```

Let's push the changes

```bash
git push -u origin feature/display-number-c
```

Let's check the list of branches we have available:

```bash
git branch
```

We got three branches: _master_, _feature/display-number-b_, _feature/display-number-c_ and the active one is _feature/display-number-c_

We got the ok from the team to merge branch _feature-display-number-b_ into _master_, let's got for it.

Let's switch to _master_ branch

```bash
git checkout master
```

Let's merge _featute/display-number-b_ it in _master_:

```bash
git merge feature/display-number-b
```

![Git merge](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-merge-branch-display-number-b.PNG)

> A good practice before merging with master is to ensure the branch is updated with the latest master cut (merge from master to our branch)

> We can do this because we go the branch available locally, if it's a branch created by another user it may happen that is available in the server but not in our local computer, we have to execute _fetch_ to bring it locally (check Misc section).

In this case there are not conflicts, we are good to go.

Let's push the updates.

```bash
git push
```

Now you can use your web browser and navigate to your provider web repository page and check that the changes are reflected, that was great!

Let's start with a hard one... we want to merge _feature/display-number-c_ into _master_ Why is a hard one? Because the version of _master_ that was taken as starting point to create _feature/display-number-c_ is different from the actual one, and it has conflicts (_index.js_ file got updated on both versions), we need to resolve the conflicts and choose what's the right piece of code to work with.

Let's try to merge _feature/display-number-c_ into _master_.

Let's ensure first that we are on _master_ branch, by executing _git branch_ command we get the list of branches available and highlighted the active branch.

```bash
git branch
```

Now let's merge this branch into _master_

```bash
git merge feature/display-number-c
```

![Merge conflicts](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-merge-branch-display-number-c-with-conflicts.PNG)

**Ouch we got merge conflicts !!** Now we have to fix them running the _git mergetool_ command.

Before running this command, let's make the following check, do you have a merge tool installed and configured? If not jump to the section **Misc/Setting up a merge tool** in this readme.

Let's run the command to start the merge:

```bash
git mergetool
```

![VSCode mergetool](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/vscode-mergetool.PNG)

Depending on your configuration a given tool will be launched (KDiff, VSCode, p4merge...), you have to go through the conflicts and choose (usually you have to choose conflict by conflict): - Whether to preserve your local changes and discard the merging branch changes. - Whether to apply your merging branch changes to master. - Apply your own code update or manual merge.

Depending on the tool it will try to auto-resolve some merge conflicts for you.

> If you don't have a mergetool configured, or want to learn more about it, check the section _Misc/Setting up a merge tool_.

Once you got all your conflicts solved, let's add them to staing and is to commit them:

```bash
git add .
```

```bash
git commit -m "fixing merge conflicts"
```

![Git commit merge](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-commit-merge-conflicts.PNG)

> Some developers may prefer to use a UI tool to manage this (e.g. Source Tree, Git Kraken, VSCode), mileage applies.

Now we can push the changes to the remote origin:

```bash
git push
```

Did you get a lot of temporary _\*.orig_ files? you can config your Git local installation by telling not to keep that files:

```bash
git config --global mergetool.keepBackup false
```

# Misc

## Create repo in our local and later on uploading to cloud repo

Some text

...

Creating repo

...

## To create and link a local repository with Github

1. You can create a new local folder for your repository or you can choose another project folder which you already wanted to upload to Github.

2. Open the terminal and locate yourself in the project folder. In order to create a new database it is necessary to write the next command-line (if you have already done this step before, the database will not be created again but reinitialized):

```bash
git init
```

3. Next, the local database must get linked with the Github repository. In order to do that, it is necessary to log in Github and create a new blank repository (without initializing it with a README file). Afterwards, you can copy the url of the repository and write it in the next command-line:

```bash
git remote add origin https://github.com/...
```

4. At the moment, the local database and the Github repository must have been linked already. Therefore, you can proceed to upload the files, but first it is necessary to put them in staging:

```bash
git add .
```

5. Now you must save the files in the local database:

```bash
git commit -m "comment"
```

6. Finally, everything is ready to upload the files to Github. However, using push is not enough since it is necessary to set the master branch of the repository first:

```bash
git push --set-upstream origin master
```

## Setting up a merge tool

Setting up a merge tool can be a tough decisition there are many available, e.g.:

- Kdiff
- VSCode
- p4Merge
- ...

In this [post](https://www.git-tower.com/blog/diff-tools-windows/) you can check tools available for Windows, in this [other](https://www.tecmint.com/best-linux-file-diff-tools-comparison/) for Linux, and for Mac Os guys, check this [post](https://www.lawtechnologytoday.org/2017/11/mac-comparing/).

[How to setup a diff tool in Mac Os](https://coderwall.com/p/3wuuda/set-diffmerge-as-default-merge-tool-in-os-x)

e.g:

Setting up Kdiff on windows:

- Install Kdiff, you can download it from [here](https://sourceforge.net/projects/kdiff3/files/latest/download)
- Open the terminal and run the following commands:

```bash
git config --global --add merge.tool kdiff3
&&
git config --global --add mergetool.kdiff3.path "C:/Program Files/KDiff3/kdiff3.exe"
&&
git config --global --add mergetool.kdiff3.trustExitCode false
&&
git config --global --add diff.guitool kdiff3
&&
git config --global --add difftool.kdiff3.path "C:/Program Files/KDiff3/kdiff3.exe"
&&
git config --global --add difftool.kdiff3.trustExitCode false
```

Setting up Kdiff on mac:

- Install Kdiff, you can download it from [here](https://sourceforge.net/projects/kdiff3/files/latest/download)
- Open the terminal and run the following commands:

```bash
git config --global --add merge.tool kdiff3
&&
git config --global --add mergetool.kdiff3.path "Applications/kdiff3.app/Contents/MacOS/kdiff3"
&&
git config --global --add mergetool.kdiff3.trustExitCode false
&&
git config --global --add diff.guitool kdiff3
&&
git config --global --add difftool.kdiff3.path "Applications/kdiff3.app/Contents/MacOS/kdiff3"
&&
git config --global --add difftool.kdiff3.trustExitCode false
```

## Playing with staging

When you are working locally in your repository, if you want to commit changes you need to perform to steps: first add the files to staging, then commit that files.

What can you do if you have added to staging a temporary file? You can remove it from stage by executing the following command:

```bash
git reset HEAD <filepath>
```

![Git reset HEAD](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-staging.PNG)

And... if you want to remove all files from staging?

```bash
git reset HEAD .
```

That was great, but what if want to discard changes on a file that was not on staging?

```bash
git checkout --
```

Last but not least... discard everything, I want to start from that fresh commit again:

```bash
git checkout HEAD
```

![Git checkout HEAD](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-checkout-head.PNG)

## Fetch

When we work on a team, there may be branches that have been uploaded to the remote server that are not avialable in our local repository, how can I download them? you can execute:

```bash
git fetch --all
```

From time to time, there are branches that may not exists anymore on the remote server but you keep them locally, how can you make some cleanup and remove those unused branches refs locally, to do that we can execute:

```bash
git fetch --prune
```

## Pull

There are times that a given person is working in the same branch you are working on and you need to pull the changes from the server branch to your branch, we cannot do this making a **fetch**, since it could generate conflicts, in that case we use **git pull**, this will download the content of the active branch from the remote server and try to merge it with your current cut.

```bash
git pull
```

## Stash

Sometime you are working in your codebase on some temporary code that you don't want to commit yet, but you need to quick switch to another branch, but... you cannot switch without committing or discarding your temporary code. Is there any way to put that temporary code "into a drawer" and once you comeback to that commit be able to restore it? That's exactly what **stash** does.

Let's create a new branch and call it _feature/saygoodbye_

```bash
git branch feature/saygoodbye
```

Let's switch into that branch

```bash
git checkout feature/saygoodbye
```

Let's update some code:

_./src/index.js_

```diff
const sampleNumber = 1;
const sampleNumberB = 2;
const sampleNumberC = 3;
console.log(`Hello number ${sampleNumber} {sampleNumberB} {sampleNumberC}`);
+ console.log('Goodbye !');
```

- We don't want to commit yet, but we need to jump into master... what can we do? Stash our changes (both staged and unstaged files will be stored locally):

```bash
git stash
```

![Git stash](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-stash.PNG)

- Now we can hop into master

```bash
git checkout master
```

- Switch back to _feature/saygoodbye_ branch.

```bash
git checkout feature/saygoobdye
```

- And recover the code that we store in stash

```bash
git stash pop
```

![Git stash pop](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-stash-pop.PNG)

> Stash store this information locally and are associated to the specific commit where _stash_ was invoked.

## Migrate repository to another one

Sometimes you need to migrate some repository to another one and you really want everything, including all branches, refs, tags, etc.

Instead of using a `standard clone`, we can follow these steps:

- Clone original repository locally `again` but with the `mirror` flag:

```bash
git clone --mirror <origin url>
```

- It will create a folder with the `repository-name.git` and it will clone the current repository stuff, something like:

![Git mirror result](./content/git-mirror.png)

- Change terminal to the new directory:

```bash
cd <cloned folder>
```

- And push everything to the new `url repository` using `mirror` flag:

```bash
git push --mirror <destination url>
```

## Cheat sheets

Some useful Github cheat sheets:

- https://i.redd.it/8341g68g1v7y.png
- https://www.git-tower.com/blog/git-cheat-sheet/
- https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf

# About Basefactor + Lemoncode

We are an innovating team of Javascript experts, passionate about turning your ideas into robust products.

[Basefactor, consultancy by Lemoncode](http://www.basefactor.com) provides consultancy and coaching services.

[Lemoncode](http://lemoncode.net/services/en/#en-home) provides training services.

For the LATAM/Spanish audience we are running an Online Front End Master degree, more info: http://lemoncode.net/master-frontend
