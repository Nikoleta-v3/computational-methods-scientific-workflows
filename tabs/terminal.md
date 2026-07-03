---
layout: workshop
title: Introduction to the command line
---

### 01 Introduction to command line

Firstly, you need to open your command line interface.

- Windows: Git bash (which was installed on your machine when you installed `git`).
- Unix (another way to describe Mac OS and/or Linux machines): we will use the
  system terminal.

### Finding your computer's name.

Let us first let's find out the name of your computer by running:

```shell
$ whoami
```

### Finding your current location

Now let's find out which directory (folder) we are currently in:

```shell
$ pwd
```
This stands for "present working directory".

Type the command in and press enter. It should list where you are currently located in your command line interface.

### Seeing what is in your current location

To view the contents of the current directory:

```shell
$ ls
```
This stands for "list".

Type the command in and press enter. You should see a list of the various files and directory in your current directory. Open your current directory in a graphical user interface and compare.

### Moving to another location

If you want to enter a directory that is in your current directory type:

```shell
$ cd <directory>
```

Try moving to your Desktop. It should be something like:

```shell
$ cd Desktop
```

### Creating a directory

To create a directory:

```shell
$ mkdir <directory_name>
```

Experiment with creating a directory for this workshop:

```shell
$ mkdir computational_methods_workshop
```
If your directory structure looked like this:

```
|--- home/
|--- Desktop/
     |--- research
     |--- photos
```

It will now look something like:

```
|--- home/
|--- Desktop/
     |--- research
     |--- photos
     |--- computational_methods_workshop
```

Move into the directory we just created:

```shell
$ cd computational_methods_workshop
```

Let's create one further directory `src`. You can navigate in the new folder with:

```shell
$ cd src
```

If you now wanted to go back to the parent directory:

```shell
$ cd ..
```

Where `..` is short hand for a previous directory.


### Creating a file

To create a file:

```shell
$ touch <file_name>
```

Experiment with creating a file named `README.md` in the directory `computational_methods_workshop`.

```shell
$ touch README.md
```

If you type `ls` you will see that the file has been created.

### Copying files

To copy a file:

```shell
$ cp <file> <new_file_directory_and_name>
```

Let's copy `README.md` to `src`:

```shell
$ cp README.md src
```

### Moving/renaming files

To move a file:

```shell
$ mv <file> <new_file_directory_and_name>
```

Having two `README.md` files could cause some confusion. So let's rename the copy we created by using the `mv` command.

```shell
$ mv src/README.md src/index.md
```

Note that if you want to rename a file you can do this by passing the new name in the same directory.

WARNING When using the command line interface you will not be prompted for
confirmation if move/mv were to overwrite another file. Be careful.

### Deleting files

To delete a file:

```shell
$ rm <file>
```

### Copying and removing directories

To copy a directory:

```shell
$ cp -r <dir> <target>
```

To remove a directory:

```shell
$ rm -r <dir>
```
