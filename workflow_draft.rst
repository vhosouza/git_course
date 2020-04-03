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