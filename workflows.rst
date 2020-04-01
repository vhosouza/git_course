Workflows
=========

`Examples of Git workflows <https://buddy.works/blog/5-types-of-git-workflows>`_

#. Simple
#. Feature branch
#. Feature branch + merge requests
#. Gitflow (Master - Develop - Feature - Feature)
#. **Forking workflow**


Working pipeline
----------------

Single-time steps:

#. Create a central group and store the main repository there
#. Each developer forks the central repository
#. Clone the forked repository to the local computer
#. Create a branch to work on an issue

Everyday steps:

#. Merge changes from upstream to your local branch
#. Make changes to the code
#. Add, commit and push to the remote fork

Detailed workflow
-----------------

In order to contribute to a third-party repository, you have to fork it, i.e. create your own copy in your GitHub account. Open web browser, log into your GitHub account, go to the repository you want to contribute, for example [https://github.com/biomaglab/tutorial-git]. Click on Fork. A repository named [https://github.com/<your_username>/tutorial-git] will be created.
Now you have to clone it to your computer to edit the code and send it back to online repository. Create a folder with na easy path, like D:\repository. Open GitShell and:

.. code-block:: sh

   cd "D:\repository"

Copy the SSH path available on the GitHub page of your fork and type on GitShell

.. code-block:: sh
    :emphasize-lines: 3,4

    git clone git@github.com:vhosouza/tutorial-git.git
    git remote -v  # List all remote repositories linked to your local copy - origin is set by default
    origin  git@github.com:vhosouza/tutorial-git.git (fetch)
    origin  git@github.com:vhosouza/tutorial-git.git (push)

Now you can work on your local copy of tutorial-git. After some programming you can add changes to a staging area. You don't have to upload every change, you can add many different files to the staging area and then upload them in separate snapshots of changes. Snapshots are the commits, and is a good practive to never lose track of your code. So after editing a lot of different files and adding them to the staging area you can create separate commits for more focused changes in specific functions to the software or algorithm. After editing and before uploading changes, you have to identify the staged, unstaged and untracked files according to your local repo and the origin repo. Use the status command.
To ignore some undesired files such as .~m or .pyc or .exe you can create a .gitignore file at the root of source code and add in each line the type of file to be igonred by git like <.pyc>.

.. code-block:: sh

    git status # Using the -s argument simplify the status description
    git add . # Adds all changes to staging area
    git add first-file.txt # Adds only changes to first-file to the staging area
    git add first-file.txt second-file.txt # Adds changes to first-file and second-file to the staging area
    git commit # This will open the text editor to write the commit description

Here, use the following style to better standardization:
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
# (use "git reset HEAD <file>..." to unstage)
#
#modified: first-line.txt
Edit the first file and create the second file

- Create a second line on first file
- Create the first line of a second file

.. code-block:: sh

    git commit -m "Your message" # You can also commit the message directly on command line

Inspecting your repository you can use:

.. code-block:: sh

    git log --oneline -n 5 # View maximum of 5 last commits in one line description
    git checkout <commit> # Point to previous commit for read-only purpose. Does not affect the code.
    git checkout <commit> <first-file.txt> # Checking out to a file directly is considered as a change to the code and will be available to be commited again. This affects the code.

It is possible to revert a specific commit. Git will apply the changes of removing a single past commit and will create a new commit with the new changes. It is a safe operation beacuse does not chenge the project history. While git reset will delete previous commits and go back to the specified commit, so it is a dangerous method beacuse modify project history. Git reset can also be used to clean the staging area from undersider commits.

.. code-block:: sh

    git revert <commit>
    git reset <commit>

To rewritte project history using branches you can use the rebase command. Basically, it puts all the commits from the branch in front of the commits of <base>. Rebase is a common way to integrate upstream changes into your local repository.

.. code-block:: sh

    git rebase <base>

Consider you are working on a branch named branch1 and after you do some programming, the master had to be modified with some fixes. To have a plain project history you must integrate the feature branch1 with a rebase.

.. code-block:: sh

    git checkout branch1
    git rebase master

This moves the branch1 commits to the tip of master, updating your branch history. Then you can merge directly to master. Now you can do a standard fast-forward merge from master, which is insert the branch1 changes into the local master.

.. code-block:: sh

    git checkout master
    git merge branch1

To use an interactive rebasing you just add the -i flag to the command. Instead of a blind set of operations the GitShell will open another session and let you pick or squash commits organizing the code.

Start branch1

.. code-block:: sh

    git checkout -b branch1 master

Edit files

.. code-block:: sh

    git commit -a -m "Start developing a feature"

Edit more files

.. code-block:: sh

    git commit -a -m "Fix something from the previous commit"

Add a commit directly to master

.. code-block:: sh

    git checkout master

Edit files

.. code-block:: sh

    git commit -a -m "Fix security hole"

Begin an interactive rebasing session

.. code-block:: sh
    
    git checkout branch1
    git rebase -i master

In another session, decide what to do with each commit:
    
.. code-block:: sh

    pick 32618c4 Start developing a feature
    squash 62eed47 Fix something from the previous commit

Now merge the branch1 into master

.. code-block:: sh

    git checkout master
    git merge branch1

To track each command done in the tip of branches use reflog:

.. code-block:: sh

    git reflog

In order to contribute to other repositories, you have to stablish remote connections. Creating a remote, you identify the url by a specific name. Using direct remote to others repositories can be useful for small teams developing large projects.

.. code-block:: sh

    git remote add upstream git@github.com:biomaglab/tutorial-git.git
    git remote add ze git@github.com:ze/tutorial-git.git

To fetch a remote repository is to import all commits and branches into the local repo.  They are stored as remote branches and is useful for reviewing changes before integrating them.

.. code-block:: sh

    git fetch ze

To synchronize the local repository with central repository master branch is the following process:

.. code-block:: sh

    git fetch origin # Download remote commits and branches
    git log --oneline master.. origin/master # See what commits were added
    git checkout master # Move to master
    git log origin/master # See what is on origin/master
    git merge origin/master # Synchronize origin/master into local master

To make things easier the pull command wrapps the fetch and merge into one command.

.. code-block:: sh
    
    git pull origin # It is the same as "git fetch origin" + "git merge origin/master"

Instead of default merge, explicit --rebase flag replaces the merge command to rebase after fetch.

To export commits to remote branch you may use push.

.. code-block:: sh

    git push origin master # Push the specified branch to the remote origin

A standard method for publishing local contributions to central repository:

.. code-block:: sh

    git checkout master
    git fetch origin master
    git rebase -i origin/master

Squash commits, fix up commit messages etc.

.. code-block:: sh

    git push origin master