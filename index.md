# How to

## GitHub
GitHub is a collection of millions of repositories that offers services to facilitate collaboration on and development of a project. GitHub offers version control, which records who made each change to a repository and when. GitHub is the largest host of source code in the world. Source code is computer instructions readable to humans, which is helpful because other users can study and further develop on their own, whereas code solely readable to computers would not provide this capability. README files provide a description of a project.

### Terminal
The command line on Terminal starts with the name of the computer followed by the name of the user. Type commands after the $. 

### Markdown
Markdown is a language and is used on GitHub mainly to write README files. A file written in markdown on GitHub is indicated by .md, for example README.md. The formatting of Markdown is as follows:
* Italic words are surrounded by _underscores_
* Bold words are surrounded by **asterisks**
* Headers come in six sizes and begin with a hashtag #, one hashtag makes the largest size and six hashtags make the smallest size
* Each item in an unordered list begins with an asterisk, and each item in an ordered list begis with a number and a period (1.) 
  * Indented items in a list need to have two spaces before the asterisk
* Paragraphs need two spaces afterwards for a soft break or a return afterwards for a hard break
* Links are made of two parts, a description written in [] and the link written in ()
* Images are the same as links with a ! in front of the brackets
* The ticks above the tab key `create monospace`

### Create Github Account
Go to [the Github website](github.com/join) and enter a username, email address, and password. 

### Create SSH Key on GitHub
SSH provides a secure channel in an unsecure network. SSH uses encryption, and on GitHub the user creates a pair of a public and private keys which will allow remote access (using the command line on Terminal). To create a SSH key on GitHub, go to settings. "SSH and GPG keys" is listed under "Personal settings" on the left side of the screen. Click "New SSH key" in the upper right corner.

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

## Continuous Integration
Continuous integration is the frequent compilation of all separate copies of a project to the main branch of a repository. Integration of a copy into the mainline can fail if continuous integration is not used because changes can be made to the mainline that the copy would not reflect. The user would then have to revise his code to update changes, which is referred to as "integration hell" because it can take a long time. Continuous integration requires frequent merging of copies with the mainline and tests for every commit so that errors can be identified and corrected immediately. Continuous Integration can be paired with continuous delivery which would make software continually available for use.

## Travis CI
Travis CI runs on GitHub - log in to Travis CI through your GitHub account and enable Travis CI builds. Each addition to code is tested by Travis CI and either passes or fails as indicated on the build status page. To run Travis CI on a repository on GitHub, add a .travis.yml file to the repository. This file details the language, what dependencies to install, what to use to do a build, and what to test against. The .travis.yml file is written in YAML format.

### Example: .travis.yml file
[Link to example](https://github.com/laurelmcintyre/python-ci/blob/master/.travis.yml)
* `language: python` means that the project is written in python.

### How to do a Build

### How to Display a Build Passing Badge on GitHub README page

## Code Coverage

## Docker
Docker is a software container platform that makes for easier collaboration between users on different devices. Docker packages the libraries and settings of a piece of software and make it work the same regardless of the device it is on.

## Doxygen
Doxygen turns code into documentation.

## SonarQube
