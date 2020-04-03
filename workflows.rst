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

In order to contribute to a third-party repository, you have to fork it, i.e. create your own copy in your GitHub account. Open web browser, log into your Aalto Version (or GitHub) account, go to the repository you want to contribute. Click on ``Fork``. A repository with the same name will be created in your own account page.

Now you have to clone it to your computer to edit the code and send it back to the online repository. Create a folder with na easy path, like ``D:\repository``. Open the terminal/shell and go to the desired folder:

.. note:: When you clone git will automatically create the folder with the repository name, so you do not need to manually create a specific folder to clone the code.

.. code-block:: sh

   cd "D:\repository"

Copy the SSH path available on the forked repository web page and past it on the terminal.

From now on, let's use as an example the username ``johnnybgood``, the repository name is ``tutorial-git``, and that the repository is on GitHub, but the same applies to Aalto Version.

.. code-block:: sh
    :emphasize-lines: 3,4

    git clone git@github.com:johnnybgood/tutorial-git.git
    git remote -v  # List all remote repositories linked to your local copy - origin is set by default
    origin  git@github.com:johnnybgood/tutorial-git.git (fetch)
    origin  git@github.com:johnnybgood/tutorial-git.git (push)

Now you can work on your local copy of tutorial-git. You will also want to be able to constantly get and make changes in the main project repository. You do that by adding a remote repository with the following command:

.. code-block:: sh

    git remote add upstream git@github.com:biomaglab/tutorial-git.git


In order to contribute to other repositories, you can keep adding these remote connections. Using direct remote to others repositories can be useful for small teams developing large projects. For instance if ``johnnybgood`` wants to contribute directly to ``marybcool``, he can add the following remote link:

.. code-block:: sh

    git remote add marybcool_repo git@github.com:marybcool/tutorial-git.git


.. note::

    ``upstream`` is a conventional name used among git users, but is just a manually set name. For instance you can name it as ``main_project``. That is why we could name the ``marybcool`` repo as ``marybcool_repo``.

You should always work on a branch other than the master. Let the branch master be just a mirror of the main repository code with the tested and stable code version. Thus, start by creating a branch named after the issue you are going to work, for instance ``new_colormap``.

.. code-block:: sh

    git checkout -b new_colormap

After you created the new branch, jump between branches using the ``checkout`` command followed by the branch name:

.. code-block:: sh

    git checkout master
    git checkout new_colormap

Now you can start the real work! After some programming you can ``add`` changes to a staging area. You don't have to upload every change, you can add many different files to the staging area and then upload them in separate snapshots of changes. Snapshots are the ``commits``, and is a good practive to never loose track of your code. So after editing a lot of different files and adding them to the staging area you can create separate commits for more focused changes in specific functions to the software or algorithm. After editing and before uploading changes, you have to identify the staged, unstaged and untracked files according to your local repo and the origin repo. Use the ``status`` command.

.. note:: 

    To ignore some undesired files such as ``.~m`` or ``.pyc`` or ``.exe`` you can create a ``.gitignore`` file at the root of source code and add in each line the type of file to be ignored by git like.

.. code-block:: sh

    git status # Using the -s argument simplify the status description
    git add . # Adds all changes to staging area
    git commit # This will open the text editor to write the commit description

.. note:: Commit messages have to be short and informative. Avoid using abstract messages such as: *Minor bugs fixed*. Later you will have a hard time to know when you made specific changes.

Another way of using the ``add`` and ``commit`` commands is:

.. code-block:: sh

    git add first-file.txt # Adds only changes to first-file to the staging area
    git add first-file.txt second-file.txt # Adds changes to first-file and second-file to the staging area
    git commit -m "FIX: Warning dialog was not showing fixed." # This commit with the specific message given and without opening the text editor

Inspecting your repository you can use:

.. code-block:: sh

    git log --oneline -n 5 # View maximum of 5 last commits in one line description
    git checkout 5a4763d5 # Move code to previous commit for read-only purpose. Does not affect the code.
    git checkout 5a4763d5 <first-file.txt> # Checking out to a file directly is considered as a change to the code and will be available to be commited again. This affects the code.

The ``5a4763d5`` is the commit specific 8-digit identifier displayed after the log command or in the repository webpage. The ``checkout`` command allows you to travel along the code history.

It is possible to revert a specific commit. Git will apply the changes of removing a single past commit and will create a new commit with the new changes. It is a safe operation beacuse does not chenge the project history. While git reset will delete previous commits and go back to the specified commit, so it is a dangerous method beacuse modify project history. Git reset can also be used to clean the staging area from undersider commits.

.. warning:: Use ``revert`` and ``reset`` commands carefully, because you can create a big mess in the code for you and all other collaborators.

.. code-block:: sh

    git revert 5a4763d5
    git reset 5a4763d5

You may want to upload your created local repository to the remote repository. Use the following command to push and track it:

.. code-block:: sh
    
    git push -u origin new_colormap

In a daily routine you will want to make changes to your code and have them available in the remote repository. Think that you will be working in the same feature on your laptop and your workstation at the University. To do so, it's best to keep the remote repository always updated. After you worked on your laptop and commited your changes, you can push them to the remote repository by:

.. code-block:: sh
    
    git push origin new_colormap

On the other day, you go to the University and will work on the workstation. If your branch is not on your workstation yet, add it by:

.. code-block:: sh
    
    git pull # this will fetch all remote branches
    git checkout new_colormap

Or:

.. code-block:: sh

    git checkout -t origin/new_colormap

If the branch is already on your computer, download the changes you made the day before using the ``pull`` command.

.. code-block:: sh
    
    git pull origin new_colormap

Now, consider that other collaborators made changes to the main repository that were already incorporated 

.. code-block:: sh

    git fetch origin # Download remote commits and branches
    git log --oneline master.. origin/master # See what commits were added
    git checkout master # Move to master
    git log origin/master # See what is on origin/master
    git merge origin/master # Synchronize origin/master into local master

To make things easier the pull command wrapps the fetch and merge into one command.

.. code-block:: sh
    
    git pull origin # It is the same as "git fetch origin" + "git merge origin/master"

.. note:: Further instructions will soon be added.