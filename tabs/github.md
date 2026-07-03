---
layout: workshop
title: GitHub
section: Collaboration
---

### 03 Collaboration using GitHub

So far, the project is tracked with git, which means you have a local history of
the changes you have made. However, it only exists on your computer.

To collaborate with others, we need a shared copy of the repository. One common
way to do this is to use GitHub.

GitHub is a website that hosts git repositories. It gives you a place to store a
copy of your repository online, share it with other people, review changes, and
collaborate through issues and pull requests.

It is not the only service that hosts git repositories, but it is a popular one.
Other services include [GitLab](https://about.gitlab.com),
[Bitbucket](https://bitbucket.org), and [SourceForge](https://sourceforge.net).

GitHub is not a replacement for git. git is the version control tool. GitHub is
one service that can host git repositories and provide collaboration features
around them.

In this section, we will publish the running example to GitHub, make a change on
a branch, open a pull request, merge it, and bring the merged changes back to
the local repository.

## Starting point

You should already have a local project called `computational_methods_workshop`
that is tracked with git.

Before connecting it to GitHub, check that your repository is clean:

```shell
$ git status
```

You should see something like:

```shell
On branch main
nothing to commit, working tree clean
```

If you have uncommitted changes, either commit them or decide that they should
not be included before continuing.

## Create a GitHub repository

1. Go to <https://github.com>.
2. Click **New repository**.
3. Choose a repository name, for example `computational_methods_workshop`.
4. Do not add a README.
5. Do not add a `.gitignore`.
6. Choose a license, for example the MIT License.

In this example we add the license on GitHub. This means the GitHub repository
will not be completely empty: it will already contain one commit adding a
`LICENSE` file.

After creating the repository, GitHub will show commands for connecting an
existing local repository.

## Connect the local repository to GitHub

In your terminal, make sure you are inside the local project folder:

```shell
$ pwd
```

If necessary, move into the project:

```shell
$ cd computational_methods_workshop
```

Connect your local repository to the GitHub repository. Replace `YOUR-USERNAME`
with your GitHub username:

```shell
$ git remote add origin git@github.com:YOUR-USERNAME/computational_methods_workshop.git
```

The name `origin` is the conventional name for the main remote copy of a
repository.

Check that the remote was added:

```shell
$ git remote -v
```

You should see `origin` listed for both fetch and push.

## Pull the license from GitHub

Because we added a license when creating the repository, GitHub already has a
commit that your local repository does not have yet.

Before pushing your local commits, bring that remote commit into your local
`main` branch:

```shell
$ git pull origin main --allow-unrelated-histories
```

The `--allow-unrelated-histories` option is needed here because the local
repository and the GitHub repository were created separately. They each started
with their own first commit.

git may open your text editor to ask for a merge commit message. The default
message is fine. Save and close the editor.

After the pull, check that the license file is now present locally:

```shell
$ ls
```

You should see `LICENSE` among the project files.

Check the status:

```shell
$ git status
```

The working tree should be clean before you continue.

## Push the local commits

Now push your local `main` branch to GitHub:

```shell
$ git push -u origin main
```

The `-u` option records that your local `main` branch should track
`origin/main`. After this, future pushes from `main` can usually be done with:

```shell
$ git push
```

Refresh the GitHub page in your browser. You should now see the project files,
the local commit history, and the `LICENSE` file.

## Make a new branch

Publishing the project does not mean that development stops. We usually avoid
working directly on `main`. Instead, we create a branch for a specific change,
push that branch, and discuss it through a pull request.

Create a new branch:

```shell
$ git checkout -b implement-source-code
```

Check where you are:

```shell
$ git status
```

The output should say:

```shell
On branch implement-source-code
```

## Add source code

Create a file called `src/compute.py`.

Add the following code:

```python
import sys
import time

if __name__ == "__main__":
    N = 1_000_000_000
    if len(sys.argv) > 1:
        N = int(sys.argv[1])

    total = 0

    start = time.perf_counter()

    for i in range(N):
        total += i

    end = time.perf_counter()

    print(f"Sum: {total}")
    print(f"Time: {end - start:.6f} s")
```

This script performs a simple computation and prints how long it took. The
default value of `N` is intentionally large, so for testing we will run it with
a smaller value.

## Test the script manually

Before committing, run the script:

```shell
$ python src/compute.py 1000000
```

If your system uses `python3`, run:

```shell
$ python3 src/compute.py 1000000
```

You should see output similar to:

```shell
Sum: 499999500000
Time: 0.085321 s
```

The exact time will be different on each computer. What matters here is that
the script runs successfully.

In a test-driven development workflow, we would turn this manual check into an
automated test before or while writing the implementation. For this workshop, we
will keep the check manual and focus on the GitHub workflow.

## Commit the change

Check what changed:

```shell
$ git status
```

Stage the new file:

```shell
$ git add src/compute.py
```

Commit it:

```shell
$ git commit -m "Add compute script"
```

Check the branch history:

```shell
$ git log --oneline --graph --decorate
```

You should see your new commit on `implement-source-code`.

## Push the branch to GitHub

Push the branch:

```shell
$ git push -u origin implement-source-code
```

GitHub now has a copy of your branch as well as `main`.

## Open a pull request

Go to the repository page on GitHub. GitHub will usually show a message
suggesting that you open a pull request for the branch you just pushed.

Click **Compare & pull request**.

In the pull request:

1. Give the pull request a clear title, for example `Add compute script`.
2. Write a short description of what changed.
3. Mention how you tested it, for example:

```text
Tested with:
python src/compute.py 1000000
```

The pull request is a place to review the proposed change before it becomes part
of `main`.

## Review and merge

On the pull request page, inspect the changed files.

When you are happy with the change, click **Merge pull request** and confirm the
merge.

After merging, GitHub's `main` branch contains the new `src/compute.py` file.

## Bring the merged changes back locally

Your local `main` branch does not automatically update when a pull request is
merged on GitHub. You need to switch back to `main` and pull the latest changes.

```shell
$ git checkout main
$ git pull
```

Check that `src/compute.py` is now present on `main`:

```shell
$ ls src
```

Run the script again:

```shell
$ python src/compute.py 1000000
```

or:

```shell
$ python3 src/compute.py 1000000
```

## Clean up the branch

Once the pull request has been merged, the feature branch is no longer needed.

Delete the local branch:

```shell
$ git branch -d implement-source-code
```

If GitHub offers to delete the remote branch after merging the pull request, you
can also delete it there.
