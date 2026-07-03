---
layout: workshop
title: Git
section: Version Control
---

### 02 Version control with Git

### Setting up git

Initially we need to set up git. 

git keeps track of the entire history of a project. This does not only mean
keeping track of what was done but also who did it. So we start by telling git
who we are by running the following two commands:

```shell
$ git config --global user.name "Your Name"
$ git config --global user.email "Your Email"
```

**Note** this is not data that is being collected by any cloud service or
similar. It just stays with your project.

**Windows** Note that all these commands work on the anaconda prompt but if you
want to use tab completion you can use the git bash command line specifically
for git.


Moreover, we are going to set [Nano](https://www.nano-editor.org) as the default
editor for git. For unix you can use the following command:

```shell
$ git config --global core.editor "nano"
```

We are going to use Nano later in order to write a commit message.

### Initialising a git repository

In order to demonstrate how version control with git works we are going to use
the `computational_methods_workshop` folder we created before.

We need tell git to start keeping an eye on this repository (folder/project).
While in the `computational_methods_workshop` directory type:

```shell
$ git init
```

You should then see a message saying that you have successfully initialized a git repository.

### Staging and committing changes

To see the status of the repository we just initialized type:
 
```shell
$ git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	README.md
	src/
```

There are various pieces of useful information here, first of all that
`README.md` and `src` are currently not tracked files.

Let's track the `README` file:

```shell
$ git add README.md
```

Run `git status again`. We see that `README.md` file is now ready to be "committed".
To commit the file we need to run the command:

```shell
$ git commit
```

When doing this, a text editor should open up prompting you to write what is called a commit message. In our case the text editor that opens is Nano. 

For the purposes of using git Nano is more than a sufficient editor, all you need to know how to do is:

```
- Write in Nano: just type;
- Save in Nano: Ctrl + O;
- Quit Nano: Ctrl + X.
```


Type the following as the first commit message:

```shell
Add README file

An empty README file. 
```

save and exit.

git should confirm that you have successfully made your first commit.

**Note** A commit message is made up of 2 main components:

```shell
<Title of the commit>

<Description of what was done>
```

- The title should be a description in the form of "if this commit is applied `<title of the commit>` will happen". The convention is for this to be rather short and to the point.
- The description can be as long as needed and should be a helpful explanation of what is happening.

A commit is a snapshot that git makes of your project, you should use this at meaningful steps of the progress of a project.

### Ignoring files

There is still a file in the repository that is currently not being tracked. This is `src/index.md`. If you are not sure why it is this file, the run the command:

```shell
$ git status -u
```

We do not want to keep tract of that files as it not related to our project.

To tell git to ignore these files we will add them to a blank file entitled `.gitignore`.

Open your editor and open a new file (`File > New file`) and type:

```shell
src/index.md
```

Save that file as `.gitignore` and then run:

```shell
$ git status 
```

We see now that `git` is ignoring the file but is aware of the `.gitignore`
file. Stage and commit the `.gitignore` file.


### Tracking changes to files

Let's assume that we want to add some information to our `README` file.

```shell
The README file explains the purpose of our project, what users can do with it, and how to use it.
```

1. Save your file and confirm that git detects the change in one file.

```shell
$ git status
```

Our file is now in the modified state, as `git status` tells us.

To see what has been modified you need to type:

```shell
$ git diff README.md
```
and press `q` to exit.

To "stage" the file for a commit we use `git add` again:

```shell
$ git add README.md
```

Now let us commit:

```shell
$ git commit
```

With the following commit message:

```
Add info to README

Added a sentence explaining the usage of a README file.
```

Finally, we can check the status: 
```shell
$ git status
```
to confirm that everything has been done correctly.    

### Exploring history

`git` allows us to visualize the history of a project and even to change it. To view the history of a repository type:

```shell
$ git log
```

This displays the full log of the project.

We see that there are <> commits there, each with a seemingly random set of numbers and characters. This set of characters is called a "hash".

The first commit with title `Add README file` has hash:
aab73629642568b9be5ca5faa5e091ea9a629d67.

**Note** that on your machines this hash will be different, in fact every hash
is mathematically guaranteed to be unique, thus it is uniquely assigned to the
changes made.

Hashes can be very useful, e.g. to re-create an earlier state of the project.

### Creating branches

Branches allow us to work on different versions of the same file in parallel, which is very important when developing software, but also when we do research.

When typing `git status` we have seen that one piece of information regularly given was:

```shell
On branch main
```

This is telling us which branch of "history" we are currently on. We can view
all branches with the command:

```shell
$ git branch
```

This shows:

```
* main
```

So currently there is only one branch called main.

Let's create a new branch named `editing-readme. To do this you need to
run the following series of commands:

```
$ git branch editing-readme
$ git checkout editing-readme
Switched to branch 'editing-readme'
```

or just 

```
$ git checkout -b editing-readme
Switched to a new branch 'editing-readme'
```

Once we are here let's add a few more sentence to our `README` file:

```
The README file explains the purpose of our project, what users can do with it, and how to use it.

We are currently not working on a project but we are developing a tutorial on Git.
```

Save and then as before, stage the file, and commit with the following commit
message

```
Add info regarding the tutorial to the README
```


###  Merging locally

The `git merge` command integrates changes from a branch into the currently
checked out branch. Before merging, make sure to have checked out the target
branch. In our example we merge changes from the "editing-readme" branch into
the "main" branch.

Before merging, always make sure you are on the *target* branch:

```shell
$ git checkout main

Switched to branch 'main'
```

Here you can check what the `README` looks like.

Now use the `git merge` command to bring in the changes from the
"add-info-readme" branch. It is good practice to always append the `--no-ff`
flag to `git merge` to create a dedicated merge commit. In this way one can
later identify changes in the code that are a result from a merge.

```shell
$ git merge editing-readme --no-ff
Updating d677093..a9c395e
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

In your editor, the commit message will already be present, you can leave it as
is or add more detailed information. Save and close the editor.

```shell
$ git log --pretty=oneline --graph --abbrev-commit

Merge branch 'editing-readme'
*   7b14540 (HEAD -> main) Merge branch 'editing-readme'
|\
| * a9c395e (editing-readme) Add info regarding the tutorial to the README
|/
* d677093 Add info to README
* 1552751 add gitignore file
* 58f3378 Add README file
```
