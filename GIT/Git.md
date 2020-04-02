Git 
====

New Git Repository for Existing Project
----

- cd to the source directory

- git init, to establish the repository

- git add .gitignore, .gitignore can be copied from previous projects

- git add *, add existing source files

- git commit -m "initial commit"

[Hookup with Git Repository on GitHub](https://help.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)
----

- create a new repository on GitHub

- Copy repository's URL

- in the local repository

- git remote add origin <the copied repository url>

- git remote -v # verifies the new remote url

- git push origin master # pushes the changes from local to GitHub