# How to
How to use GitHub, Travis CI, CodeCov, SonarQube, and Docker with c repository as example -- refer to [documentation_python.md](https://github.com/laurelmcintyre/documentation/blob/gh-pages/instructions_python.md) for more detailed instructions

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
    * Add to .travis.yml file

            env:
              matrix:
                - DISTRO=ubuntu
                - DISTRO=ubuntu COVERAGE=ON

            script:
              - clang -coverage -O0 helloworld.c -o hello
              - ./hello
              - gcov helloworld.c

            after_success:
              - bash <(curl -s https://codecov.io/bash)
    * Go to [the CodeCov website](http://codecov.io/), log in, and add the new repo -- then the codecoverage dashboard should appear
    * Go to settings and copy the markdown version of the badge to post on the README file
    
8. SonarQube
    * Log in to SonarQube and generate a new token under account settings -- put the token in environmental variables on Travis
    * Add to .travis.yml file
    
            addons:
              sonarqube:
              organization: "<org-name>-github"


            script:
              - sonar-scanner
              
9. Doxygen 
[Using Doxygen with Travis](https://gist.github.com/vidavidorra/548ffbcdae99d752da02)
