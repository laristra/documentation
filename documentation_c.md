# How to
How to use GitHub, Travis CI, CodeCov, SonarQube, and Doxygen using c with [example c repository](https://github.com/laurelmcintyre/c)

## GitHub
[GitHub](github.com/) is a collection of millions of repositories that offers services to facilitate collaboration on and development of a project. GitHub offers version control, which records who made each change to a repository and when. GitHub is the largest host of source code in the world. Source code is computer instructions readable to humans, which is helpful because other users can study and further develop on their own. README files provide a description of a project.

### The Shell and Terminal
The shell is a program used on the command-line interface (on Terminal) to read commands and run other programs. The command line on Terminal starts with the name of the computer followed by the name of the user. Type commands after the $. 

### Git Commands in Terminal
`git config -h` show list of GitHub commands

`git config --list` list settings for GitHub account

`git init` make directory on computer a GitHub repo

`git status` status of a project

`git add <file_name>` add file from computer to GitHub, needs to be committed and pushed

`git commit - m "<message>"` add commit to GitHub

`git push` push repo to GitHub

`git log` shows history of project

`git diff` difference between current file and last saved file, can be edited to show difference between two chosen files 

`git checkout` restore old version of a file, also can be used to switch branches in a repo

`git pull` pull updated files from GitHub to computer

`git clone` clone repo

### Commands in Terminal
`mkdir <name>` make directory on computer

`cd <directory_name>` go into directory

`ls` list contents of directory

`cat <file_name>` display contents of file in Terminal

`touch <file_name>` make new file

`rm <file_name>` remove file

`nano <file_name>` create and/or edit a file, use ctrl-o to save and ctrl-x to exit

`cp <file1_name> <file2_name>` copy one file into another

`*.txt` select all files ending with text (can be any ending)

`echo` returns input as output, i.e. returning value of variable

`clear` clear Terminal window

`$VAR` stores value of variable 

### Markdown
Markdown is a language used on GitHub mainly to write README files. A file written in markdown on GitHub is indicated by .md, such as README.md. The formatting of Markdown is as follows:
* Italic words are surrounded by _underscores_
* Bold words are surrounded by **asterisks**
* Headers come in six sizes and begin with a hashtag #, one hashtag makes the largest size and six hashtags make the smallest size
* Each item in an unordered list begins with an asterisk, and each item in an ordered list begins with a number and a period (1.) 
  * Indented items in a list need to have two spaces before the asterisk
* Paragraphs need two spaces afterwards for a soft break or a return afterwards for a hard break
* Links are made of two parts, a description written in [] and the link written in ()
* Images are the same as links with a ! in front of the brackets
* The tick above the tab key `creates monospace`

### Create Github Account or Create new GitHub Repo
1. Go to [the Github website](github.com/join) and enter a username, email address, and password. 
2. Go to [the Github website](github.com/join) and add a new repo--give it a name and initialize it with a README file.

### Create SSH Key on GitHub -- Only once per account
SSH provides a secure channel in an unsecure network. SSH uses encryption, and on GitHub the user creates a pair of public and private keys which allows remote access (using the command line on Terminal). To [create a SSH key for GitHub](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/), go to Terminal. Type `ssh-keygen -t rsa -b 4096 -C "<your_email>"`. Press enter to save. Do not enter a passphrase. Then, go to GitHub settings. "SSH and GPG keys" is listed under "Personal settings" on the left side of the screen. Click "New SSH key" in the upper right corner. Copy the public version of the SSH key into the window on GitHub. [More instructions](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

### Clone GitHub Repo to your computer
[Github's instructions](https://help.github.com/articles/cloning-a-repository/) are fairly straightforward -- first click "Clone or Download" on the main page of a repo and copy the SSH key version. Then go to Terminal and type `git clone https://github.com/<username>/<repo_name>`, which will clone the contents of the git repo to your computer. 
 
### Create a new file on your repo
Create a program/function. Travis, CodeCov, SonarQube, etc. will all test this file(s). To pull the file from GitHub to your local computer, type `cd <repo_name>`, which will navigate into the repo and `git pull` which will pull the updated files on the repo from GitHub to the computer.

## Continuous Integration
Continuous integration is the frequent compilation of all separate copies of a project to the main branch of a repository. Integration of a copy into the mainline can fail if continuous integration is not used because changes can be made to the mainline that the copy would not reflect. The user would then have to revise his or her code to update changes, which is referred to as "integration hell" because it can take a long time. Continuous integration requires frequent merging of copies with the mainline and tests for every commit so that errors can be identified and corrected immediately. Continuous Integration can be paired with continuous delivery which would make software continually available for use.

## Travis CI
[Travis CI](http://travis-ci.org/) can run on GitHub — log in to Travis CI through your GitHub account and enable Travis CI builds. Each addition to code is tested by Travis CI and either passes or fails as indicated on the build status page. To run Travis CI on a GitHub repository, add a .travis.yml file to the repository. This file details the language of the project, what dependencies to install, what to use to do a build, and what to test against. The .travis.yml file is written in YAML format. Once the .travis.yml file is configured correctly on GitHub, run `git push origin master` on Terminal to trigger the first build, and then Travis CI will run builds after every commit to your GitHub repository.

### Example of a .travis.yml file
[Link to example c repository](https://github.com/laurelmcintyre/c)
* `language: c` means that the project is written in c.
*  `script: ${CC} <file-name>.c -o <file-name>` runs the compiler gcc or clang on a file
*  Specify the compiler which the script runs on

        compiler:
          - gcc
          - clang 
          
* To start the first build, go to Travis and switch the tab from off to on on the new repo. The first build should start eventually.

### How to Display Build Passing Badge on GitHub
A [badge](https://github.com/laurelmcintyre/c/blob/master/README.md) displays the status of your build from Travis CI onto your GitHub page. To display the badge on a README page, go to Travis. By the account name should be the build passing badge. Click on it and a window will pop up. Change the setting to Markdown and copy and paste the link it generates into the README page.

## Create a Deploy Key for the Repo
In place of a personal access token which can be used to access all of an organization's repos, a deploy key (SSH key) is specific to one repo and therefore is safer. To create a deploy key, go to Terminal. 

    * `git pull` makes sure the repo is up to date on the local machine
    * `ssh-keygen -f deploy` to generate a deploy key
    * Do not enter a passphrase, hit enter
    * `cat deploy.pub` .pub indicates the public key and cat displays the content of the public deploy key in terminal
    * Go to GitHub settings and click Add Deploy Key
    * Copy the content from `cat deploy.pub` into the window on GitHub that "Add Deploy Key" opens and click "Allow write access"
    
    Now, you will add the private key, encrypted, to GitHub, which travis commands in Terminal automatically generate
    * `travis encrypt-file ./deploy` tells travis to encrypt the private deploy key
    * type yes when prompted for detected repository name
    * `travis encrypt-file --add after success ./deploy` tells to add in the after_success portion of the .travis.yml file
    * type yes when prompted to override deploy.enc
    * `git add deploy.enc` .enc implies the encrypted version of the deploy key, add this to github
    * `git add .travis.yml` the .travis.yml file now has the encrypted key on it, so add this to github as well
    * `git commit -m "encrypt deploy key"
    * Edit the .travis.yml file (`nano .travis.yml`) in terminal and add the following in after_success:
    
          mv deploy ~/.ssh/id-rsa

          chmod 600 ~/.ssh/id-rsa
       
      The first command means the deploy key will be in the ssh folder and the second chmod 600 command means that only you have read/write access and no one else. The chmod command is in the XYZ format, with X meaning you, Y meaning your group, and Z meaning everyone else. The numbers range from 0 - 7 and have different meanings, such as 2 means write and 4 means read, so 6 = 2 + 4 and has both these permissions.
      Save the changes with Ctrl o, hit enter, and then hit Ctrl x to exit nano.
    * `git add .travis.yml` add the travis file again
    * `git commit -m "add .travis.yml`
    * `git push` push changes to github

## Cache
Caches store data to speed up processes, for example, requests are temporarily stored so that the same request later could be served faster. Travis CI [caches dependencies and directories](https://docs.travis-ci.com/user/caching/) which makes the build  go quicker. To enable caching in Travis, add the following lines to the .travis.yml file.

    before_install:
      - ccache -z
      
    after_success:
      - ccache -s
      
    cache:
      ccache: true
      
## Code Coverage
[Code coverage](http://codecov.io/) shows what percent of code is being tested by Travis in builds. A high percentage is ideal to guard against bugs. To create a code coverage account, log in through GitHub and click "Add Repository." Codecov provides a token for uploading reports which is unnecessary for Travis CI. Go to account settings on GitHub, install CodeCov, and under "Configure," add the new repo to "Repository Access" and hit save. Then [configure the .travis.yml file](https://docs.codecov.io/docs)(below), [add a .codecov.yml file](https://docs.codecov.io/v4.3.6/docs/codecov-yaml)(below), and the code coverage account page will be working.

### Additions to .travis.yml file needed for Code Coverage
      sudo: required
      
      dist: trusty
      
      script:
        - ${CC} --coverage -c <file_name>.c -o <file_name>.o
        - ${CC} --coverage <file_name>.o -o <file_name>
        - ./<file_name>
      after_success:
        - if [ ${CC} = clang ]; then
            bash <(curl -s https://codecov.io/bash) -F ${CC} --gcov-exec "llvm-cov gcov";
          else
            bash <(curl -s https://codecov.io/bash) -F ${CC};
          fi
          
### Add .codecov.yml file
The .codecov.yml file controls the settings for CodeCov, and is used for customization if you don't want to use the default.
A .codecov.yml file can look like this:

      coverage:
        precision: 1
        round: down
      range: "70...100"

### How to Display Code Coverage Badge on Github
Under settings on the Code Coverage website, click on "Badge." Copy the markdown version and paste it in the README file on GitHub. 

## SonarQube
SonarQube is meant to improve code quality. It progresses through a series of conditions (the default conditions/setting can be set) which must all be met in order for a project to pass. For example, in order to pass, an example project must have code coverage greater than 80% and a maintainability rating, reliability rating, and security rating all equal to A. This default setting applies to all future projects unless changed. SonarQube checks for bugs, vulnerabilities, code smells (parts of code which indicate bigger, underlying problem with the code), and duplications. To make a SonarQube account, log in through your GitHub account. Then, go to "My Account" in SonarQube, click on "Security," and "Generate Token." Go to Travis project settings and enter the token into Environmental Variables and name it. Travis has an [instruction page](https://docs.travis-ci.com/user/sonarqube/) on how to configure the .travis.yml file. The file will require the organization key, which can be found under your username on the Account Settings on sonarcloud (it should be username-github). You will need to add these lines to your .travis.yml file:

      addons:
        sonarcloud:
          organization: "<username>-github"

      script:
        - sonar-scanner

      cache:
        ccache: true
        directories:
          - $HOME/.sonar
          
Then, make a sonar-project.properties file:
    
      sonar.projectKey=<project_name>:master

      sonar.projectName=<project_name>


      sonar.projectVersion=1.0

      sonar.sources=.


      sonar.sourceEncoding=UTF-8

### How to Post Quality Gate Badge on GitHub
In Markdown, the format for a Quality Gate Badge is `[![Quality Gate](https://sonarqube.com/api/badges/gate?key=<project-key>)](https://sonarqube.com/dashboard/id=<project_key>)`. 

## Doxygen 
[Doxygen](http://www.stack.nl/~dimitri/doxygen/) is a tool for generating documentation for code in several different languages, mainly c and c++. The documentation can be displayed on a webpage browser. Download Doxygen to your computer, go to a directory, and run `doxygen -g`. This will create a Doxyfile. Then `open Doxyfile` to get the template and standard settings for a Doxyfile. Put this in your GitHub repo as DOXYFILE. The only things that necessarily need to be changed are PROJECT_NAME and INPUT (INPUT, if using the shell file (below), should be set equal to ../..). Next, create a gh-pages branch of the repo by going to the repo settings, and under GitHub Pages it should say "Source" -- click on it and switch the branch to master.

The first step to run Doxygen is to create a shell file which could be called generateDocumentationAndDeploy.sh. The .travis.yml file will reference this file, but having a separate shell file means that all of this source code does not have to be in .travis.yml. The TRAVIS_REPO_SLUG variable refers to <username>/<repo_name>, so it does not have to be changed for every project. 

      echo 'Setting up the script...'

      set -e

      mkdir code_docs
      cd code_docs

      git clone -b gh-pages https://github.com/${TRAVIS_REPO_SLUG}
      cd ${TRAVIS_REPO_SLUG##*/}

      git config --global push.default simple
      git config user.name "Travis CI"
      git config user.email "travis@travis-ci.org"

      rm -rf *

      echo "" > .nojekyll

      echo 'Generating Doxygen code documentation...'

      doxygen $DOXYFILE 2>&1 | tee doxygen.log

      if [ -d "html" ] && [ -f "html/index.html" ]; then

          echo 'Uploading documentation to the gh-pages branch...'

          git add --all

          git commit -m "Deploy code docs to GitHub Pages Travis build: ${TRAVIS_BUILD_NUMBER}" -m "Commit: ${TRAVIS_COMMIT}"


          git push --force "git@github.com:${TRAVIS_REPO_SLUG}" > /dev/null 2>&1
      else
          echo '' >&2
          echo 'Warning: No documentation (html) files have been found!' >&2
          echo 'Warning: Not going to push the documentation to GitHub!' >&2
          exit 1
      fi

The .travis.yml requires changes as well. Doxygen will not generate documentation for the gh-pages branch because it is where the auto-generated files from Doxygen are pushed to. The addons install packages for doxygen to run and the after_success refers to the generateDocumentationAndDeploy.sh file which was just made -- the `if [[ ${TRAVIS_JOB_NUMBER} = *.1 ]]` means that in a build matrix, Doxygen will only generate documentation on the first build. 
    
      branches:
        except:
          - gh-pages

      env:
        global:
          - DOXYFILE: $TRAVIS_BUILD_DIR/DOXYFILE

      addons:
        apt:
          packages:
            - doxygen
            - doxygen-doc
            - doxygen-latex
            - doxygen-gui
            - graphviz

      after_success:
        - chmod +x generateDocumentationAndDeploy.sh
        - if [[ ${TRAVIS_JOB_NUMBER} = *.1 ]]; then ./generateDocumentationAndDeploy.sh; fi
         
To display some form of documentation, add comments to the top of the file being documented. For example, for a simple hello world function, the comment could look like this, which would be published as "a helloworld program in c":

      /// \file

      /** \brief a helloworld program in c
       */

Doxygen documentation should publish on the gh-pages branch. To find the documentation displayed on a web browser, go into repo settings and click the link under "Your site is published at https://<username>.github.io/<repo_name>/". Add html after <repo_name>/ to see the published documentation.
