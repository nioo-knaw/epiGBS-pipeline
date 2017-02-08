Project skeleton
================

A skeleton Git repository for analysis projects.


Starting a project
------------------

To start a new analysis project based on this skeleton, follow the following steps:

1. Fork the skeleton repository.

        Click on the Fork button or go to https://gitlab.bioinf.nioo.knaw.nl/nioo-bioinformatics/nioo-project-skeleton/forks/new

1. Rename the project.

        Click on the settings icon (top right) and select Edit Project
        Scroll down to the Rename repository section and give your project a new name

1. Review the other settings

        On the same page you can also choose to make your project public or not. Public repositories can be cloned by anyone, even without a NIOO account. By default, your repository will be internal.


1. In the *Settings* tab, click *Members* to add people that work on the same project.

1. Create a local copy of the repository on the NIOO server

        Advanced users can select the SSH option but beginners can better change it to HTTPS. Copy the url and run on your target location on the server:
        git clone <your project url>

Please start by replacing the contents of this README file with the appropriate content for your project.


Project structure
-----------------

Ideally, every directory in the project has a README file containing enough information for new people to be able to interpret what's in the directory and to work with it.


### The toplevel `README` file

This file contains general information about the project, for example:

- Who leads the project.
- Who participates in the project.
- The amount of hours people have spent on this project.


### The `doc` directory

Documentation on the project, such as annotated sample lists, goal of the project, related work and literature.


### The `data` directory

Used to store all raw (i.e., delivered by the scientist or from elsewhere. Please provide additional notes in a README file.

Also see *Working with large files* below.


### The `analysis` directory

All analysis is stored here, including run scripts, Make files and result files.

Please try to separate self-contained parts of the analysis in their own subdirectories and document dependencies in a README file.

Also see *Working with large files* below.


### The `src` directory

Any custom scripts and specific software versions for this project can be stored here. But please consider moving things to their own repository when you think they might be useful for other projects as well.


Working with large files
------------------------

Since storage on our GitLab server and desktop computers is limited, it is not a good idea to track very large files directly in the repository. However, we do want to have some way to track our input and output data (which is usually quite large). A solution for this is provided by [git-annex](http://git-annex.branchable.com/).

git-annex allows you to manage large files with git without storing their contents in git. It works by storing file checksums in git instead of their contents.

To track a large file using git-annex, use `git annex add <filename>`. You can now do `git commit` as normal.

For more information, please see the [git-annex walkthrough](http://git-annex.branchable.com/walkthrough/) or the [gitlab documentation](http://docs.gitlab.com/ee/workflow/git_annex.html#using-gitlab-git-annex)

Publishing data at Zenodo or Figshare and obtaining a citable DOI
-----------------------------------------------------------------
Have a look at [Git-RDM](https://github.com/ctjacobs/git-rdm), a Research Data Management (RDM) plugin for the Git

Working together on the same clone
----------------------------------

If you need to work with other people on the same repository clone on the Shark cluster, you can use the following command to give group access:

    find -type d -exec chmod 775 {} \;
    find -type f -exec chmod 664 {} \;

Cloning to a portable disk (NTFS)
---------------------------------

Presuming your disk is attached to your own computer and the repository is on
the Shark cluster, you can use the following commands.

    git clone ssh://shark/path/to/project/my-project
    cd my-project

For some reason `annex-ignore` is set to `true`, we need to set it to `false`.

    git config remote.origin.annex-ignore false

Set the repository to *direct mode* to prevent making symlinks, otherwise it
will be very difficult for windows users to access the files.

    git annex direct

SSH caching seems to be broken on NTFS.

    git config annex.ssh-options "-S /tmp/ssh_mux_%h_%p_%r"

Or alternatively, disable SSH caching all together (slow when working with a
large number of files).

    git config annex.sshcaching false

Get the content.

    git annex get .
