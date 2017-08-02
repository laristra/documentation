# How to
How to use GitHub, Travis CI, CodeCov, SonarQube, Pydoc, and Docker with [python repository](https://github.com/laurelmcintyre/python-ci) as example

## GitHub
[How to set up GitHub account, create SSH key for GitHub, and clone repository to local computer](https://github.com/laurelmcintyre/documentation/blob/gh-pages/github_instructions.md)

### The Shell and Terminal
[Shell/Terminal description and Terminal commands](https://github.com/laurelmcintyre/documentation/blob/gh-pages/shell_and_terminal.md)

### Markdown
[Markdown formatting description](https://github.com/laurelmcintyre/documentation/blob/gh-pages/markdown.md)

## Travis CI
[Continuous Integration description, Travis CI setup, build passing badge](https://github.com/laurelmcintyre/documentation/blob/gh-pages/travis_ci.md)

## Example of a .travis.yml file
[Link to example python-ci repository](https://github.com/laurelmcintyre/python-ci/blob/master/.travis.yml)
* `language: python` specifies the language of the repo as python
* Put in the version of python that Travis will run if other than python 2.7 (the default). Two or more versions creates a build matrix and runs two or more different builds.

      python:
        - "2.7"
        - "3.2"
      
* `script: echo hello world` — a specific script is not necessary at this point, but the build will fail without a script
* [Further instructions](https://docs.travis-ci.com/user/languages/python/#Specifying-Test-Scriptz)

## How to Resolve a Merge Conflict
[How to resolve a merge conflict when merging two copies of a branch](https://github.com/laurelmcintyre/documentation/blob/gh-pages/resolve_merge_conflict.md)

## Create a Personal Access Token OR Create a Deploy Key for the Repo
[Create personal access token](https://github.com/laurelmcintyre/documentation/blob/gh-pages/gh_personal_access_token.md) OR [Create an SSH deploy key](https://github.com/laurelmcintyre/documentation/blob/gh-pages/deploy_key.md) -- SSH deploy key is more secure

## Code Coverage
[CodeCov, .codecov.yml file, and code coverage badge](https://github.com/laurelmcintyre/documentation/blob/gh-pages/codecov.md)

### Additions to .travis.yml file needed for Code Coverage
    sudo: required
Sudo: required is necessary to install codecov. This specifies a [trusty build environment](https://docs.travis-ci.com/user/trusty-ci-environment).

    before_install:
       - sudo pip install codecov
The sudo: required setting allows pip to install codecov.

    script:
      - coverage run <file>
      - codecov
Run coverage on specified file(s) and run codecov.
* Codecoverage has [additional instructions](https://docs.codecov.io/docs)
          
## SonarQube
[SonarCloud, sonar-project.properties file, quality gate badge](https://github.com/laurelmcintyre/documentation/blob/gh-pages/sonarcloud.md)

You will need to add these lines to your .travis.yml file:

    addons:
      sonarcloud:
        organization: "<organization_key>"

    script:
      - sonar-scanner

### Pip
Pip can be used to install software packages written in Python. You can use pip in Terminal to install, upgrade, or uninstall packages using the following syntax (or with `pip install --user <package>`:
`pip install <package>`
`pip install -- upgrade <package>`
`pip uninstall <package>`

Pip is also used in the .travis.yml file under before_install to use codecov: 
`pip install codecov`

## Pydoc
Pydoc creates documentation for python files. Documentation specifies the functions in a file, what they do, and more generally what the program is for so that others can understand the purpose of the project. To generate documentation for a python file from Terminal, navigate to the repo using the cd command. Then type `pydoc -w <file>`. Make sure to leave .py off the file. For example, to find the mytan.py file, I would type `pydoc -w mytan`. To see the documentation in Terminal, type `pydoc <file>`. To show the documentation on a webpage, type `pydoc -p 0`. Copy and paste the link it generates into a browser search window.

### How to Upload Pydoc Documentation to GitHub using Travis
The GH_REPO_TOKEN which you created earlier is used to upload Pydoc documentation to GitHub. Add the following lines to your .travis.yml file:

    after_success: |
      #<token_name> should be GH_REPO_TOKEN
      if [ -n "$<token_name>" ]; then
        cd "$TRAVIS_BUILD_DIR"
        pydoc -w $TRAVIS_BUILD_DIR
        #makes directory called docs and moves html documentation there
        mkdir docs
        mv *.html docs/.
        cd docs
        git init
        #move to gh-pages branch
        git checkout -b gh-pages
        git add .
        git -c user.name='travis' -c user.email='travis' commit -m init
        #push documentation to gh-pages branch
        git push -f https://<user_name>:$<token_name>@github.com/<user_name>/<project_name> gh-pages 
      fi
      
After this builds on Travis, go to the home page of your repo, click on "branch," and switch from master to gh-pages. The html files should be in the gh-pages branch. Then go to the repo settings. Under "GitHub Pages" it should say "Your site is published at <link>". To see the documentation, go to the link and after the existing link, type the file name you would like to see documentation for. For example, if the link is originally https://laurelmcintyre.github.io/python-ci/, I would change it to https://laurelmcintyre.github.io/python-ci/main.html in order to see the documentation for main.py.

## Nose 2
[Nose 2](http://nose2.readthedocs.io/en/latest/) is testing for python.
Add the following lines to your .travis.yml file to test with Nose 2:

    before_install:
    - sudo pip install nose2 nose2-cov
    
    script:
    - nose2 --with-coverage <file>
    
## Docker
Docker is a software container platform packages the libraries and settings of a piece of software and makes it perform the same regardless of the device it is on. To use Docker, make an account and enter a username, email, and password. Go to DockerHub settings under "linked accounts and services" and link GitHub. Then, create a Dockerfile, which can look like this:
    #builds from python:2.7 image
    FROM python:2.7

    COPY . /<repo_name>

This is a very simple Dockerfile and does very little -- most of the commands are still run in .travis.yml. However, this shows how to set up Docker. The [ccmake-docker repo documentation](https://github.com/laurelmcintyre/documentation/blob/gh-pages/documentation_ccmake-docker.md) has more details on how to configure a GitHub repo to use Docker and Docker has more instructions on [how to build a Dockerfile](https://docs.docker.com/engine/reference/builder/#run).  
Go back to Docker settings and click [Create an Automated Build](https://hub.docker.com/add/automated-build/github/), enter your GitHub username and project name and click "Create." Adjust the .travis.yml file to use [Docker in Travis builds](https://docs.travis-ci.com/user/docker/) by adding the following lines:

    sudo: required
  
    services:  
      - docker

    env:
      global:
        - COMMIT=${TRAVIS_COMMIT::8}
        - REPO=<username>/<project_name>
       
    before_install:
     - docker build -t $REPO:$COMMIT -t $REPO:latest .     

    script:
      - docker run -it $REPO:$COMMIT /bin/bash -c "cd <project_name>"
