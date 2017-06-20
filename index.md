# How to
Overview

## GitHub
[GitHub](github.com/) is a collection of millions of repositories that offers services to facilitate collaboration on and development of a project. GitHub offers version control, which records who made each change to a repository and when. GitHub is the largest host of source code in the world. Source code is computer instructions readable to humans, which is helpful because other users can study and further develop on their own, whereas code solely readable to computers would not provide this capability. README files provide a description of a project.

### Terminal
The command line on Terminal starts with the name of the computer followed by the name of the user. Type commands after the $. 

#### Short List of Commands in Terminal
* `ls` shows everything in the current folder
* `cd` change directory
* `git clone` clone repo on GitHub — all commands for GitHub begin with `git`
* `git status`
* `git add`
* `git log`
* `touch <file>` creates new file
* `git push`
* `git commit`
* `git pull`
* `git diff` 

### Markdown
Markdown is a language and is used on GitHub mainly to write README files. A file written in markdown on GitHub is indicated by .md, such as README.md. The formatting of Markdown is as follows:
* Italic words are surrounded by _underscores_
* Bold words are surrounded by **asterisks**
* Headers come in six sizes and begin with a hashtag #, one hashtag makes the largest size and six hashtags make the smallest size
* Each item in an unordered list begins with an asterisk, and each item in an ordered list begins with a number and a period (1.) 
  * Indented items in a list need to have two spaces before the asterisk
* Paragraphs need two spaces afterwards for a soft break or a return afterwards for a hard break
* Links are made of two parts, a description written in [] and the link written in ()
* Images are the same as links with a ! in front of the brackets
* The tick above the tab key `creates monospace`

### Create Github Account
Go to [the Github website](github.com/join) and enter a username, email address, and password. 

### Create SSH Key on GitHub
SSH provides a secure channel in an unsecure network. SSH uses encryption, and on GitHub the user creates a pair of public and private keys which allow remote access (using the command line on Terminal). To create a SSH key on GitHub, go to settings. "SSH and GPG keys" is listed under "Personal settings" on the left side of the screen. Click "New SSH key" in the upper right corner.

### Make a Github Page and Clone it to your Computer
 * Create a new repository and name it `username.github.io`, substituting your username
 * Open Terminal and type in the location of where you want to file your repository
   * For example, to put the repository in documents, type `cd Documents` into Terminal
 * Type in git clone and then the link, which can be found under "clone or download" on the repository page
 * Type `cd username.github.io` which will move the location to your project on your computer
 * Type:
`git add --all`
`git commit - m "initial commit"`
`git push -u origin master`
 * [Github pages site](https://pages.github.com/)

## Continuous Integration
Continuous integration is the frequent compilation of all separate copies of a project to the main branch of a repository. Integration of a copy into the mainline can fail if continuous integration is not used because changes can be made to the mainline that the copy would not reflect. The user would then have to revise his or her code to update changes, which is referred to as "integration hell" because it can take a long time. Continuous integration requires frequent merging of copies with the mainline and tests for every commit so that errors can be identified and corrected immediately. Continuous Integration can be paired with continuous delivery which would make software continually available for use.

## Travis CI
[Travis CI](http://travis-ci.org/) can run on GitHub — log in to Travis CI through your GitHub account and enable Travis CI builds. Each addition to code is tested by Travis CI and either passes or fails as indicated on the build status page. To run Travis CI on a GitHub repository, add a .travis.yml file to the repository. This file details the language of the project, what dependencies to install, what to use to do a build, and what to test against. The .travis.yml file is written in YAML format. Once the .travis.yml file is configured correctly on GitHub, Travis CI will run builds after every commit to your GitHub repository.

### Example of a .travis.yml file
[Link to example python-ci repository](https://github.com/laurelmcintyre/python-ci/blob/master/.travis.yml)
Python projects require a script key or else the build will fail. 
* `language: python` means that the project is written in python.
* `script: echo hello world` — a specific script is not necessary at this point, but the build will fail without a script
* [Further instructions](https://docs.travis-ci.com/user/languages/python/#Specifying-Test-Scriptz)

### How to Display Build Passing Badge on GitHub
A badge displays the status of your build from Travis CI onto your GitHub page.
To display the badge a README page, go to Travis. By the account name should be the build passing badge. Click on it and a window will pop up. Change the setting to Markdown and copy and paste the link it generates into the README page.
[Examples of badges](https://github.com/laurelmcintyre/python-ci/blob/master/README.md)

## Code Coverage
[Code coverage](http://codecov.io/) shows what percent of code is being tested by Travis in builds. A high percentage is ideal to guard against bugs. To create a code coverage account, log in through GitHub and click "Add Repository." Codecov provides a token for uploading reports which is unnecessary for Travis CI. Go to account settings on GitHub, install CodeCov, and under "Configure," add a new repo to "Repository Access" and hit save. Then configure the .travis.yml file (below) and the code coverage account page will be working.

### Additions to .travis.yml file needed for Code Coverage
    sudo: required
Sudo: required is necessary to install codecov and also installs Docker as a service. This specifies a [trusty build environment](https://docs.travis-ci.com/user/trusty-ci-environment).

    before_install:

       - sudo pip install codecov
The sudo: required setting allows pip to install codecov.

    script:
 
      - coverage run <file>
  
      - codecov
 
* Codecoverage has [additional instructions](https://docs.codecov.io/docs)

### Pip
Pip can be used to install software packages written in Python. You can use pip in Terminal to install, upgrade, or uninstall packages using the following syntax:
`pip install <package>`
`pip install -- upgrade <package>`
`pip uninstall <package>`

Pip is also used in the .travis.yml file under before_install to use codecov: 
`pip install codecov`

### How to Display Code Coverage Badge on Github
Under settings on the Code Coverage website, click on "Badge." Copy the markdown version and paste it in the README file on GitHub. 

## How to Resolve a Merge Conflict
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

To resolve, choose which of the two options to delete and keep the other, using `git commit` and `git push`.

## Pydoc
Pydoc creates documentation for python files. To generate documentation for a python file from Terminal, navigate to the directory with the file (using the cd command). Then type in `pydoc -w <file>`. Make sure to leave .py off the file. For example, to find the mytan.py file, I would type `pydoc -w mytan`. To see the documentation in Terminal, type `pydoc <file>`, which would be `pydoc mytan` in the case of the example. To show the documentation on a webpage. type in `pydoc -p 0`. Copy and paste the link it generates into a browser search window.

## SonarQube
SonarQube is meant to improve code quality. It progresses through a series of conditions (the default conditions/setting can be set) which must all be met in order for a project to pass. For example, in order to pass, the python-ci project must have code coverage greater than 80% and a maintainability rating, reliability rating, and security rating all equal to A. This default setting applies to all future projects unless changed. SonarQube checks for bugs, vulnerabilities, code smells (parts of code which indicate bigger, underlying problem with the code), and duplications. To make a SonarQube account, log in through your GitHub account. Then, go to "My Account" in SonarQube, click on "Security," and "Generate Token." Go to Travis project settings and enter the token into Environmental Variables and name it. Travis has an [instruction page](https://docs.travis-ci.com/user/sonarqube/) on how to configure the .travis.yml file. The file will require the organization key, which can be found under your username on the Account Settings on sonarcloud (it should be username-github). You will need to add these lines to your .travis.yml file:

    addons:
    
    sonarqube:
    
      organization: "organization-key"
  
    script:

      - sonar-scanner
  
View the [python-ci .travis.yml](https://github.com/laurelmcintyre/python-ci/blob/master/.travis.yml) file for an example. SonarQube also requires a sonar-project.properties file, which will have the following lines of code:

    sonar.projectKey=project-key

    sonar.projectName=project-name

    sonar.projectVersion=1.0

    sonar.sources=.

    sonar.sourceEncoding=UTF-8

### How to Post Quality Gate Badge on GitHub
In Markdown, the format for a Quality Gate Badge is [![Quality Gate](https://sonarqube.com/api/badges/gate?key=<project-key>)](https://sonarqube.com/dashboard/id=<project-key>). View the example at the [python-ci README page](https://github.com/laurelmcintyre/python-ci/blob/master/README.md). Clik on the raw README page to see the format.

## Nose 2
[Nose 2](http://nose2.readthedocs.io/en/latest/)

## Docker
Docker is a software container platform packages the libraries and settings of a piece of software and make it work the same regardless of the device it is on.
To use [Docker in Travis builds](https://docs.travis-ci.com/user/docker/), add `services: -docker` in your .travis.yml file. Builds must run on trusty to use Docker, so add `sudo: required` to your .travis.yml file.
[Docker Instructions](https://docs.docker.com/get-started/)
