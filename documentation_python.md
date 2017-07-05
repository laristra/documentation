# How to
How to use GitHub, Travis CI, CodeCov, SonarQube, Pydoc, and Docker with [python repository](https://github.com/laurelmcintyre/python-ci) as example

## GitHub
[GitHub](github.com/) is a collection of millions of repositories that offers services to facilitate collaboration on and development of a project. GitHub offers version control, which records who made each change to a repository and when. GitHub is the largest host of source code in the world. README files provide a description of a project.

### The Shell and Terminal
The shell is a program used on the command-line interface (on Terminal) to read commands and run other programs. The command line on Terminal starts with the name of the computer followed by the name of the user. Type commands after the $. 

### Git Commands in Terminal
`git config -h` show list of GitHub commands  
`git config --list` list settings for GitHub account  
`git init` make directory on computer a GitHub repo  
`git clone` clone GitHub repo to computer  
`git status` show status of a project  
`git add <file_name>` add file from computer to GitHub, needs to be committed and pushed  
`git commit - m "<message>"` add commit to GitHub, needs to be pushed  
`git push` push repo to GitHub, final step  
`git log` shows history of project by commit  
`git diff` difference between current file and last saved file, can be edited to show difference between two chosen files   
`git checkout` restore old version of a file or switch branches  
`git pull` pull updated files from GitHub to computer  

### Commands in Terminal
`mkdir <name>` make directory  
`cd <directory_name>` navigate into directory  
`ls` list contents of directory  
`cat <file_name>` display contents of file in Terminal  
`touch <file_name>` make new file  
`rm <file_name>` remove file  
`nano <file_name>` create and/or edit a file, use ctrl-o to save and ctrl-x to exit  
`cp <file1_name> <file2_name>` copy one file into another  
`*.txt` select all files ending with txt (can be any ending)  
`echo` returns input as output, i.e. returning value of variable  
`clear` clear Terminal window  
`$VAR` stores value of variable  

### Markdown
Markdown is a language used on GitHub mainly to write README.md files. A file written in markdown on GitHub is indicated by .md. The formatting of Markdown is as follows:
* _Italic_ words are surrounded by underscores
* **Bold** words are surrounded by two asterisks on either side
* Headers come in six sizes and begin with a hashtag #, one hashtag makes the largest size and six hashtags make the smallest size
* Each item in an unordered list begins with an asterisk, and each item in an ordered list begins with a number and a period 
  * Indented items in a list have two spaces before the asterisk
* Paragraphs need two spaces afterwards for a soft break or a return afterwards for a hard break
* Links are made of two parts, a description written in [] and the link contained in ()
* Images are formatted the same as links with a ! in front of the first brackets
* The tick above the tab key `creates a monospace`

### Create Github Account
Go to [the Github website](github.com/join) and enter a username, email address, and password. 

### Create SSH Key on GitHub -- Only once per account
SSH provides a secure channel in an unsecure network using encryption. On GitHub, the user creates a pair of public and private keys which allows remote access to a repo using the command line on Terminal. To [create a SSH key for GitHub](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/), go to Terminal. Type `ssh-keygen -t rsa -b 4096 -C "<your_email>"`. Press enter to save. Do not enter a passphrase. Then, go to GitHub settings. "SSH and GPG keys" is listed under "Personal settings" on the left side of the screen. Click "New SSH key" in the upper right corner. Copy the public version of the SSH key into the window on GitHub. [More instructions](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

### Make a Github Pages Site and Clone it to your Computer
 * [Github pages site](https://pages.github.com/)
 * Create a new repository and name it `<username>.github.io`, substituting your username
 * Open Terminal and type in the location of where you want to file your repository
   * For example, to put the repository in Desktop, type `cd Desktop` on the command line
 * Type in `git clone` and then the SSH version of the link, which can be found under "clone or download" on the repository page
 * Type `cd <username>.github.io` which will navigate into the local GitHub repository on the computer
 * Type:
`git add --all`
`git commit -m "initial commit"`
`git push -u origin master` -- Adding updated files from a computer to GitHub requires these three steps every time.

### Clone a GitHub Repo to your computer
[Github's instructions](https://help.github.com/articles/cloning-a-repository/) are fairly straightforward -- first click "Clone or Download" on the main page of a repo and copy the SSH key version. Then go to Terminal and type `git clone <SSH_key>`, which will clone the contents of the git repo to your computer. To navigate into this repo, type `cd <repo_name>`.

### Create a new file on your repo
Create a program in a new file. Travis, CodeCov, SonarQube, etc. will all test this file (or files). To pull the file from GitHub to your local computer, type `cd <repo_name>`, which will navigate into the repo and `git pull` which will pull the updated files on the repo from GitHub to the computer.

## Continuous Integration
Continuous integration is the frequent compilation of all separate copies of a project to the main branch of a repository. Integration of a copy into the mainline can fail without continuous integration because changes can be made to the main branch after the copy is made that the copy would not reflect. The user would then have to revise his or her code to update changes, which is referred to as "integration hell" because it can take a long time. Continuous integration requires frequent merging of copies with the main branch and tests for every commit so that errors can be identified and corrected immediately.

### How to Resolve a Merge Conflict
One of the reasons for continuous integration is to avoid merge conflict. However, if two people edit copies of a file and attempt to merge them into the main branch, they will have to fix the conflicts. For example, part of the mytan.py file was originally like this:

    if __name__ == "__main__":

        arg = sys.argv[1].
        
        print "Tan of %s is %s" % (arg, float(mytan(arg)))
        
    
Edits were made on two separate copies of the mytan.py file. The first was this:

    if __name__ == "__main__":  `

        print "Tan of %s is %s" % (arg, mytan(float(sys.argv[1])))`
    
The second was this:

    if __name__ == "__main__":`

        arg=float(sys.argv[1])`
    
        print "Tan of %s is %s" % (arg, mytan(arg))`

Integration caused a merge conflict that looked like this:

    if __name__ == "__main__":`

    <<<<<<< HEAD`

        print "Tan of %s is %s" % (arg, mytan(float(sys.argv[1])))`
  
    =======`

        arg=float(sys.argv[1])`
   
        print "Tan of %s is %s" % (arg, mytan(arg))`
    
    >>>>>>> 9d7863ddb992bc06aa9d243225ff875e7c15b3f8`

To resolve, choose which of the two options to delete and keep the other, using `git add`, `git commit`, and `git push`.

## Travis CI
[Travis CI](http://travis-ci.org/) can run on GitHub — log into Travis CI through your GitHub account and enable Travis CI builds. Each addition to code is tested by Travis CI and either passes or fails as indicated on the build status page. To run Travis CI on a GitHub repository, add a .travis.yml file to the repo. This file details the language of the project, what dependencies to install, what to use to do a build, and what to test against. The .travis.yml file is written in YAML format. Once the .travis.yml file is configured correctly on GitHub and the tab for the repo is switched to on in Travis settings, Travis will run builds after every commit to the GitHub repo.

### Example of a .travis.yml file
[Link to example python-ci repository](https://github.com/laurelmcintyre/python-ci/blob/master/.travis.yml)
* `language: python` specifies the language of the repo as python
* Put in the version of python that Travis will run if other than python 2.7 (the default). Two or more versions creates a build matrix and runs two or more different builds.

      python:
        - "2.7"
        - "3.2"
      
* `script: echo hello world` — a specific script is not necessary at this point, but the build will fail without a script
* [Further instructions](https://docs.travis-ci.com/user/languages/python/#Specifying-Test-Scriptz)

### How to Display Build Passing Badge on GitHub
A badge ([![Build Status](https://travis-ci.org/laurelmcintyre/python-ci.svg?branch=master)](https://travis-ci.org/laurelmcintyre/python-ci)) displays the status of your build from Travis CI. To display the badge on a README file, go to Travis. By the account name should be the build passing badge. Click on it and a window will pop up. Change the setting to Markdown and copy and paste the link it generates into the README page.

## Create a Personal Access Token
A personal access token allows for remote access to a repo. A more secure alternative is a SSH Deploy Key (below), but this example uses a personal access token. To create a personal access token, go to GitHub settings, generate a personal access token, and click public_repo. Copy it under Travis environmental variables and call it GH_REPO_TOKEN.

## OR Create a Deploy Key for the Repo
In place of a personal access token which can be used to access all of an organization's repos, a deploy key (SSH key) is specific to one repo and therefore is safer. To create a deploy key, go to Terminal. 
* `git pull` makes sure the repo is up to date on the local machine  
* `ssh-keygen -f deploy` to generate a deploy key  
* Do not enter a passphrase, hit enter  
* `cat deploy.pub` .pub indicates the public key and cat displays the content of the public deploy key in terminal  
* Go to GitHub settings and click "Add Deploy Key"
* Copy the content from `cat deploy.pub` into the window on GitHub that "Add Deploy Key" opens and click "Allow write access"  
* `travis encrypt-file ./deploy` tells travis to encrypt the private deploy key  
* type yes when prompted for detected repository name  
* `travis encrypt-file --add after success ./deploy` tells to add in the after_success portion of the .travis.yml file  
* type yes when prompted to override deploy.enc  
* `git add deploy.enc` .enc implies the encrypted version of the deploy key, add this to github  
* `git add .travis.yml` the .travis.yml file now has the encrypted key on it, so add this to github as well  
* `git commit -m "encrypt deploy key"`  
* Edit the .travis.yml file (`nano .travis.yml`) in terminal and add the following in after_success:  
    
      mv deploy ~/.ssh/id-rsa

      chmod 600 ~/.ssh/id-rsa

The first command means the deploy key will be in the ssh folder and the second chmod 600 command means that only you have read/write access and no one else. The chmod command is in the XYZ format, with X pertaining to your access rights, Y meaning your group, and Z meaning everyone else. The numbers range from 0 - 7 and have different meanings. 2 means write and 4 means read, so 6 = 2 + 4 and has both these permissions.

* Save the changes with Ctrl o, hit enter, and then hit Ctrl x to exit nano.  
* `git add .travis.yml` add the travis file again  
* `git commit -m "add .travis.yml`  
* `git push` push changes to github  

## Code Coverage
[Code coverage](http://codecov.io/) shows what percent of code is being tested by Travis in builds. A high percentage is ideal to guard against bugs. To create a CodeCov account, log in through GitHub and click "Add Repository." Codecov provides a token for uploading reports which is unnecessary for Travis CI. Go to account settings on GitHub, install CodeCov, and under "Configure," add a new repo to "Repository Access" and hit save. Then configure the .travis.yml file (below) and the .codecov.yml file and the code coverage account page will be working.

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

### Add .codecov.yml file
The [.codecov.yml](https://docs.codecov.io/v4.3.6/docs/coverage-configuration) file controls the settings for CodeCov.
A .codecov.yml file can look like this:

      coverage:
        precision: 1
        round: down
      range: "70...100"
      
Range specifies the code coverage range in percent corresponding to color. The low number, in this case 70, means that code coverage under or equal to 70% would show a red background. 100% would be green, and there would be a range of colors inbetween. Depending on the project, different ranges of code can be expected, so 100% does not necessarily have to be the top number if it is unattainable. 

Round specifies how the code percentage should be rounded, whether up, down, or nearest.

Precision specifies how many decimal points CodeCov will round the percent to. precision: 1 means that the percentage would be rounded to the tenth, precision: 2 would round to the hundredth, and so on.

These three settings are the minimum configuration, but there are more [optional settings](https://docs.codecov.io/v4.3.6/docs/codecov-yaml) that you can add to specify the configuration of CodeCov for your project.

### How to Display Code Coverage Badge on Github
Under settings on the Code Coverage website, click on "Badge." Copy the markdown version and paste it in the README file on GitHub. 

### Pip
Pip can be used to install software packages written in Python. You can use pip in Terminal to install, upgrade, or uninstall packages using the following syntax:
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

## SonarQube
SonarQube is meant to improve code quality. It progresses through a series of conditions (the default conditions can be set) which must all be met in order for a project to pass. For example, in order to pass, the python-ci project must have code coverage greater than 80% and a maintainability rating, reliability rating, and security rating all equal to A. This default setting applies to all future projects unless changed. SonarQube checks for bugs, vulnerabilities, code smells (parts of code which indicate bigger, underlying problem with the code), and duplications. To make a SonarQube account, log in through your GitHub account. Then, go to "My Account" in SonarQube, click on "Security," and "Generate Token." Go to Travis project settings and enter the token into Environmental Variables and name it SONARQUBE_TOKEN. Travis has an [instruction page](https://docs.travis-ci.com/user/sonarqube/) on how to configure the .travis.yml file for SonarCloud. The file will require the organization key, which can be found under your username on the Account Settings on SonarCloud (it should be username-github). You will need to add these lines to your .travis.yml file:

    addons:
      sonarcloud:
        organization: "<organization_key>"

    script:
      - sonar-scanner
  
View the [python-ci .travis.yml](https://github.com/laurelmcintyre/python-ci/blob/master/.travis.yml) file for an example. SonarQube also requires a sonar-project.properties file, which will have the following default lines of code:

    sonar.projectKey=<project_key>

    sonar.projectName=<project_name>

    sonar.projectVersion=1.0

    sonar.sources=.

    sonar.sourceEncoding=UTF-8

### How to Post Quality Gate Badge on GitHub
In Markdown, the format for a Quality Gate Badge is `[![Quality Gate](https://sonarqube.com/api/badges/gate?key=<repo_name>%3Amaster)](https://sonarqube.com/dashboard/id=<repo_name>%3Amaster)`. Type it into the README file.

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
