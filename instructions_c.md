# How to
How to use GitHub, Travis CI, CodeCov, SonarQube, and Docker with c repository as example -- refer to [instructions_python.md](https://github.com/laurelmcintyre/documentation/blob/gh-pages/instructions_python.md) for more detailed instructions

1. Create new Github repo

  * Go to [the Github website](github.com/join) and add a new repo--give it a name and initialize it with a README file.

2. Clone GitHub repo to your Computer

  * [Github Instructions](https://help.github.com/articles/cloning-a-repository/)

3. Create a new file on your repo

  * Enter a simple function for Travis, CodeCov, etc. to run on

4. Pull file from GitHub to computer

  * `cd <repo-name>`

  * `git pull`

5. Make a .travis.yml file 

    language: c
    
    sudo: required
    
    script: gcc <file-name>.c -o hello
    
    compiler:
      - gcc
      - clang
    
  * Go to Travis and switch the tab from off to on on the new repo. To trigger the first Travis build, make sure the repo is up to date on your local computer and type `git push origin master`. The first build should start.

6. Put Travis Badge on GitHub

  * Next to the repo name on Travis should be a build badge. Click on it and copy the link in Markdown format to paste on the repo's README page.

7. CodeCov
