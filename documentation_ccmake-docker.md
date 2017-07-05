# How to
How to use GitHub, Travis CI, CodeCov, SonarQube, Doxygen, CMake, and Docker using c with [example ccmake-docker repository](https://github.com/laurelmcintyre/ccmake-docker) 
This repo goes through the same steps as the cpp-cmake repo. If you want to create the repo with Docker from the start, read the Docker instructions (bottom of this file) first.

## GitHub
[GitHub](github.com/) is a collection of millions of repositories that offers services to facilitate collaboration on and development of a project. GitHub offers version control, which records who made each change to a repository and when. GitHub is the largest host of source code in the world. Source code is computer instructions readable to humans, which is helpful because other users can study and further develop on their own. README files provide a description of a project.

### The Shell and Terminal
The shell is a program used on the command-line interface (on Terminal) to read commands and run other programs. The command line on Terminal starts with the name of the computer followed by the name of the user. Type commands after the $. 

### Git Commands in Terminal
`git config -h` show list of GitHub commands

`git config --list` list settings for GitHub account

`git init` make directory on computer a GitHub repo

`git status` status of a project

`git add <file_name>` add file from computer to GitHub - needs to be committed and pushed

`git commit - m "<message>"` add commit to GitHub

`git push` push repo to GitHub

`git log` shows history of project

`git diff` difference between current file and last saved file, can be edited to show difference between chosen files 

`git checkout` restore old version of a file, also can be used to change branches in a repo

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

`echo` returns input as output

`sort` order list alphabetically

`uniq` filter out duplicate lines

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
[Github's instructions](https://help.github.com/articles/cloning-a-repository/) are fairly straightforward -- first click "Clone or Download" on the main page of a repo and copy the SSH key version. Then go to Terminal and type `git clone <SSH_key>`, which will clone the contents of the git repo to your computer. 
 
### Create a new file on your repo
Create a program/function. Travis, CodeCov, SonarQube, etc. will all test this file(s). To pull the file from GitHub to your local computer, type `cd <repo_name>`, which will navigate into the repo and `git pull` which will pull the updated files on the repo from GitHub to the computer.

## Continuous Integration
Continuous integration is the frequent compilation of all separate copies of a project to the main branch of a repository. Integration of a copy into the mainline can fail if continuous integration is not used because changes can be made to the mainline that the copy would not reflect. The user would then have to revise his or her code to update changes, which is referred to as "integration hell" because it can take a long time. Continuous integration requires frequent merging of copies with the mainline and tests for every commit so that errors can be identified and corrected immediately. Continuous Integration can be paired with continuous delivery which would make software continually available for use.

## Travis CI
[Travis CI](http://travis-ci.org/) can run on GitHub — log in to Travis CI through your GitHub account and enable Travis CI builds. Each addition to code is tested by Travis CI and either passes or fails as indicated on the build status page. To run Travis CI on a GitHub repository, add a .travis.yml file to the repository. This file details the language of the project, what dependencies to install, what to use to do a build, and what to test against. The .travis.yml file is written in YAML format. Once the .travis.yml file is configured correctly on GitHub, run `git push origin master` on Terminal to trigger the first build, and then Travis CI will run builds after every commit to your GitHub repository.

### Example of a .travis.yml file
[Link to example c repository](https://github.com/laurelmcintyre/c)
* `language: c` means that the project is written in c.
*  `script: ${CC} <file-name>.c -o <file-name>` runs the compiler on a file -- the ${CC} variable is replaced by the specified compilers, gcc and clang, and runs two versions of each build
*  Specify the compiler which the script runs on

        compiler:
          - gcc
          - clang 
          
* To start the first build, go to Travis and switch the tab from off to on on the new repo. The first build should start eventually.

### How to Display Build Passing Badge on GitHub
A [badge](https://github.com/laurelmcintyre/c/blob/master/README.md) displays the status of your Travis CI build on your GitHub page. To display the badge on a README page, go to Travis. By the account name should be the build passing badge. Click on it and a window will pop up. Change the setting to Markdown and copy and paste the link it generates into the README page.

## Create a Deploy Key for the Repo
In place of a personal access token which can be used to access all of an organization's repos, a deploy key (SSH key) is specific to one repo and therefore is safer. To create a deploy key, go to Terminal. 

* `git pull` makes sure the repo is up to date on the local machine  
* `ssh-keygen -f deploy` to generate a deploy key  
* Do not enter a passphrase, hit enter  
* `cat deploy.pub` .pub indicates the public key and cat displays the content of the public deploy key in terminal  
* Go to GitHub settings and click Add Deploy Key  
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

The first command means the deploy key will be in the ssh folder and the second chmod 600 command means that only you have read/write access and no one else. The chmod command is in the XYZ format, with X meaning you, Y meaning your group, and Z meaning everyone else. The numbers range from 0 - 7 and have different meanings, such as 2 means write and 4 means read, so 6 = 2 + 4 and has both these permissions.

* Save the changes with Ctrl o, hit enter, and then hit Ctrl x to exit nano.  
* `git add .travis.yml` add the travis file again  
* `git commit -m "add .travis.yml`  
* `git push` push changes to github  

## OR Create a Personal Access Token
To create a personal access token, which is less secure option than the Deploy SSH key, go to GitHub settings, generate a personal access token, and click public_repo. Copy it under Travis environmental variables and call it SONARQUBE_GITHUB_TOKEN.

## Cache
Caches store data to speed up processes, for example, requests are temporarily stored so that the same request later could be served faster. Travis CI [caches dependencies and directories](https://docs.travis-ci.com/user/caching/) which makes the build  go quicker. To enable [ccache](https://ccache.samba.org/), add the following lines to the .travis.yml file.

    #install ccache in before_install, then run it if the build passes

    before_install:
      - ccache -z
      
    after_success:
      - ccache -s
      
    cache:
      ccache: true
      
## Code Coverage
[Code coverage](http://codecov.io/) shows what percent of code is being tested by Travis in builds. A high percentage is ideal to guard against bugs. To create a code coverage account, log in through GitHub and click "Add Repository." Codecov provides a token for uploading reports which is unnecessary for Travis CI. Go to account settings on GitHub, install CodeCov, and under "Configure," add the new repo to "Repository Access" and hit save. Then [configure the .travis.yml file](https://docs.codecov.io/docs)(below), [add a .codecov.yml file](https://docs.codecov.io/v4.3.6/docs/codecov-yaml)(below), and CodeCov will be working.

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

## SonarQube
SonarQube is meant to improve code quality. It progresses through a series of conditions (the default conditions can be set) which must all be met in order for a project to pass. For example, in order to pass, a default project must have code coverage greater than 80% and a maintainability rating, reliability rating, and security rating all equal to A. This default setting applies to all future projects unless changed. SonarQube checks for bugs, vulnerabilities, code smells (parts of code which indicate bigger, underlying problem with the code), and duplications. To make a SonarQube account, log in through your GitHub account. Then, go to "My Account" in SonarQube, click on "Security," and "Generate Token." Go to Travis project settings and enter the token into Environmental Variables and name it SONARQUBE_TOKEN. Travis has an [instruction page](https://docs.travis-ci.com/user/sonarqube/) on how to configure the .travis.yml file. The file will require the organization key, which can be found under your username on the Account Settings on sonarcloud (it should be username-github). You will need to add these lines to your .travis.yml file:

      addons:
        sonarcloud:
          organization: "<username>-github"

      script:
        - sonar-scanner

      cache:
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
[Doxygen](http://www.stack.nl/~dimitri/doxygen/) is a tool for generating documentation for code in several different languages, mainly c or c++. The documentation can be displayed on a webpage browser. Download Doxygen to your computer, navigate to a directory, and run `doxygen -g`. This will create a Doxyfile. Then `open Doxyfile` to get the template and standard settings for a Doxyfile. Put this in your GitHub repo as DOXYFILE. Most of the configurations do not need to be changed, but make sure to change the project name to your repo name. Also, make sure `INPUT = ../..` and `GENERATE_HTML = YES`. Next, create a gh-pages branch of the repo by going to the repo settings, and under "GitHub Pages" it should say "Source" -- click on it and switch the branch to master.

The first step to run Doxygen is to create a shell file which could be called generateDocumentationAndDeploy.sh. The .travis.yml file will reference this file, but having a separate shell file means that all of this source code does not have to be in .travis.yml. The ${TRAVIS_REPO_SLUG} variable refers to <username>/<repo_name>, so it does not have to be changed for every project. 

      echo 'Setting up the script...'

      set -e
      
      #make a directory called code_docs and navigaate to it
      mkdir code_docs
      cd code_docs

      #clone the gh-pages branch of the repo and navigate there
      git clone -b gh-pages https://github.com/${TRAVIS_REPO_SLUG}
      cd ${TRAVIS_REPO_SLUG##*/}

      git config --global push.default simple
      git config user.name "Travis CI"
      git config user.email "travis@travis-ci.org"

      rm -rf *

      echo "" > .nojekyll

      echo 'Generating Doxygen code documentation...'

      doxygen $DOXYFILE 2>&1 | tee doxygen.log

      #if the files are generated in html, add them to GitHub
      if [ -d "html" ] && [ -f "html/index.html" ]; then

          echo 'Uploading documentation to the gh-pages branch...'

          git add --all

          git commit -m "Deploy code docs to GitHub Pages Travis build: ${TRAVIS_BUILD_NUMBER}" -m "Commit: ${TRAVIS_COMMIT}"

      #force push the documentation to GitHub -- the gh-pages branch gets rewritten with every commit
      #if one of the above conditions is not met, the documentation will not push to GitHub
          git push --force "git@github.com:${TRAVIS_REPO_SLUG}" > /dev/null 2>&1
      else
          echo '' >&2
          echo 'Warning: No documentation (html) files have been found!' >&2
          echo 'Warning: Not going to push the documentation to GitHub!' >&2
          exit 1
      fi
    
The .travis.yml requires changes as well:
    
      #travis will ignore the gh-pages branch because the auto-generated Doxygen files are pushed there
      branches:
        except:
          - gh-pages
     
      env:
        global:
          - DOXYFILE: $TRAVIS_BUILD_DIR/DOXYFILE
          
      #install doxygen packages
      addons:
        apt:
          packages:
            - doxygen
            - doxygen-doc
            - doxygen-latex
            - doxygen-gui
            - graphviz
            
      #refers to shell file that was just made
      #only run the shell file if it is the first build, aka the first compiler
      after_success:
        - chmod +x generateDocumentationAndDeploy.sh
        - if [[ ${TRAVIS_JOB_NUMBER} = *.1 ]]; then ./generateDocumentationAndDeploy.sh; fi
         
To display some form of documentation, add comments to the top of the file being documented. For example, for a simple hello world function, the comment could look like this, which would be published as "a helloworld program in c":

      /// \file

      /** \brief a helloworld program in c
       */

Doxygen documentation should publish on the gh-pages branch. To see the documentation displayed on a web browser, go into repo settings and click the link under "Your site is published at https://<username>.github.io/<repo_name>/". Add html after <repo_name>/ to see the published documentation.


## CMake
[CMake](https://cmake.org/overview/) is a system that manages builds. To enable CMake, you will need a CMakeLists.txt file and edits to your .travis.yml file. Below is a simple CMakeLists.txt file that defines the project, executable, and target. 

      cmake_minimum_required (VERSION 2.6)
      project (<repo_name>)
      add_executable(<repo_name> <file_name>.c)

      set_property(TARGET <repo_name> PROPERTY CXX_STANDARD 11)
      set_property(TARGET <repo_name> PROPERTY CXX_STANDARD_REQUIRED ON)
      
To add Doxygen commands to the MakeFile instead of the .travis.yml file, add the following after the above text.

      find_package(Doxygen)
      if (DOXYGEN_FOUND)
        add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/html/index.html
          COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/DOXYFILE
          DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/DOXYFILE
          COMMENT "Build doxygen documentation")
        add_custom_target(html DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/html/index.html)
      endif()
      
      option(ENABLE_COVERAGE_BUILD "Do a coverage build" ON)
      
Finally, to configure the .travis.yml file correctly, add this to the .travis.yml file and replace the existing script with the following script (leaving sonar-scanner in place).

      addons:
        apt:
          packages:
            - cmake-data
            - cmake

      script:
        - cmake . -DENABLE_COVERAGE_BUILD=ON
        - make
        - make html

## Docker
Docker is a software container platform packages the libraries and settings of a piece of software and makes it perform the same regardless of the device it is on. To use Docker, make an account and enter a username, email, and password. Go to DockerHub settings under "linked accounts and services" and link GitHub. 

First, you must create a build environment, which can be in a separate repo such as [buildenv](https://github.com/laurelmcintyre/buildenv) or on a separate branch of the same repo. The Dockerfile will refer to the build environment and pull from it. This example uses [Fedora](https://www.docker.com/docker-fedora) and Ubunutu. In the buildenv repo, there is a .travis.yml file, a fedora file, and an ubuntu file.
.travis.yml:

    language: c 

    sudo: required

    services:
     - docker

    env:
      matrix:
        - DISTRO=fedora DOCKERHUB=true
        - DISTRO=ubuntu DOCKERHUB=true

    script:
      - mkdir ${HOME}/docker
      - cp -v ${DISTRO} ${HOME}/docker/Dockerfile
      - if [[ ${TRAVIS_BRANCH} != master ]]; then TAG="${TAG}_${TRAVIS_BRANCH}"; fi
      - docker build -t ${TRAVIS_REPO_SLUG}:${DISTRO}${TAG} ${HOME}/docker/

    after_success:
      - shopt -s extglob && [[ ${TRAVIS_BRANCH} = @(master|refactor) ]] && DEPLOY=yes
      - if [[ ${DOCKERHUB} = true && ${DOCKER_USERNAME} && ${DOCKER_PASSWORD} && ${TRAVIS_PULL_REQUEST} == false && ${DEPLOY} ]]; then
          docker login -u="$DOCKER_USERNAME" -p="$DOCKER_PASSWORD";
          docker push "${TRAVIS_REPO_SLUG}:${DISTRO}${TAG}";
    fi

fedora:
    #pulls from the latest image of fedora
    FROM fedora:latest
    RUN dnf install -y make cmake git gcc-c++ gcc-gfortran flex patch doxygen graphviz pandoc python2 openmpi-devel exodusii-devel cereal-devel lapack-devel scotch-devel metis-devel environment-modules python-pip clang llvm compiler-rt ccache texlive-epstopdf-bin ghostscript-core texlive-latex-bin-bin texlive-collection-fontsrecommended texlive-fancyhdr findutils texlive-booktabs gdb wget curl lcov
    RUN wget -O /usr/bin/doxy-coverage https://raw.githubusercontent.com/alobbs/doxy-coverage/master/doxy-coverage.py
    RUN chmod +x /usr/bin/doxy-coverage

    RUN groupadd -r user
    RUN useradd -r -m -g user user
    USER user
    ENV PATH=/usr/lib64/ccache:${PATH}${PATH:+:}/usr/lib64/openmpi/bin/
    ENV LD_LIBRARY_PATH=${LD_LIBRARY_PATH}${LD_LIBRARY_PATH:+:}/usr/lib64/openmpi/lib/
    #ENV MPI_INCLUDE=/usr/include/openmpi-x86_64
    #ENV MPI_LIB=/usr/lib64/openmpi/lib
    ENV PYTHONPATH=/usr/local/lib/python2.7/site-packages${PYTHONPATH:+:}${PYTHONPATH}
    WORKDIR /home/user
    RUN pip install --user codecov coverxygen
    
ubuntu:

    FROM ubuntu:latest
    RUN apt-get -q update -y
    RUN apt-get -qq install -y make cmake cmake-data git g++ gfortran flex doxygen graphviz pandoc python2.7 libopenmpi-dev libcereal-dev liblapacke-dev libexodusii-dev libscotch-dev libmetis-dev python-pip texlive-font-utils clang llvm ccache texlive-latex-base texlive-fonts-recommended texlive-latex-recommended gdb wget curl lcov
    RUN wget -O /usr/bin/doxy-coverage https://raw.githubusercontent.com/alobbs/doxy-coverage/master/doxy-coverage.py
    RUN chmod +x /usr/bin/doxy-coverage

    RUN apt-get install -y openjdk-8-jdk unzip software-properties-common python-software-properties
    RUN wget https://sonarsource.bintray.com/Distribution/sonar-scanner-cli/sonar-scanner-2.8.zip https://sonarqube.com/static/cpp/build-wrapper-linux-x86.zip
    RUN unzip sonar-scanner-2.8.zip -d /sonarqube/
    RUN unzip build-wrapper-linux-x86.zip -d /sonarqube/
    ENV PATH=${PATH}${PATH:+:}/sonarqube/build-wrapper-linux-x86:/sonarqube/sonar-scanner-2.8/bin

    RUN groupadd -r user
    RUN useradd -r -m -g user user
    USER user
    ENV PATH=/usr/lib/ccache:${PATH}
    WORKDIR /home/user
    RUN pip install --user codecov coverxygen

In the repo, delete the sonar-project.properties file and the generateDocumentationAndDeploy.sh file. The .travis.yml file, CMakeLists.txt file, and DOXYFILE will be edited. Much of the travis commands are placed instead in the Dockerfile. Therefore, the commands are all in the same place and now can be built from any machine because of Docker. Travis variables/environmental variables are also transferred to Docker. Four environmental variables will need to be set in Travis or else the build will fail because the variables will not exist: DOCKER_USERNAME, DOCKER_PASSWORD, SONARQUBE_GITHUB_TOKEN (github repo token), and SONARQUBE_TOKEN (instructions above under SonarQube on how to generate token in SonarCloud). This example also uses a SSH deploy key, which is automatically put under Travis environmental variables. The other variables below, i.e. ${CC} or ${TRAVIS_JOB_NUMBER}, are autogenerated by Travis.
The new .travis.yml looks like this:

     language: c

     sudo: required
     
     #run docker
     services:
       - docker
       
     #runs 8 different builds--scripts with the variables COVERAGE, SONARQUBE, etc. will only run when set to ON
     env:
       matrix:
         - DISTRO=ubuntu
         - DISTRO=ubuntu COVERAGE=ON SONARQUBE=ON

         - DISTRO=fedora DOCKERHUB=ON
         - DISTRO=fedora COVERAGE=ON

     script:
       - cp -vr docker ${HOME}/docker
       - sed -i "1s/fedora/${DISTRO}/" ${HOME}/docker/Dockerfile
       #navigate to parent directory
       - cd ../../
       - mv -v ${TRAVIS_REPO_SLUG} $HOME/docker/git-src
       - cp -r $HOME/.ccache ${HOME}/docker/ccache
       - cp -r $HOME/.sonar ${HOME}/docker/sonar
       # transfer travis variables to docker
       - docker build --build-arg COVERAGE=${COVERAGE}
                     --build-arg CC=${CC} --build-arg CXX=${CXX}
                     --build-arg SONARQUBE_GITHUB_TOKEN=${SONARQUBE_GITHUB_TOKEN}
                     --build-arg SONARQUBE=${SONARQUBE} --build-arg SONARQUBE_TOKEN=${SONARQUBE_TOKEN}
                     --build-arg TRAVIS_BRANCH=${TRAVIS_BRANCH} --build-arg TRAVIS_JOB_NUMBER=${TRAVIS_JOB_NUMBER}
                     --build-arg TRAVIS_PULL_REQUEST=${TRAVIS_PULL_REQUEST} --build-arg TRAVIS_JOB_ID=${TRAVIS_JOB_ID}
                     --build-arg TRAVIS_TAG=${TRAVIS_TAG} --build-arg TRAVIS_REPO_SLUG=${TRAVIS_REPO_SLUG}
                     --build-arg CI=${CI} --build-arg TRAVIS=${TRAVIS} --build-arg TRAVIS_OS_NAME=${DISTRO}
                     --build-arg TRAVIS_COMMIT=${TRAVIS_COMMIT}
                     -t ${TRAVIS_REPO_SLUG}:latest ${HOME}/docker/ &&
         rm -rf ${HOME}/.ccache &&
         CON=$(docker run -d ${TRAVIS_REPO_SLUG}:latest /bin/bash) &&
         docker cp ${CON}:/home/user/.ccache ${HOME}/ &&
         docker cp ${CON}:/home/user/.sonar ${HOME}/

     after_success:
       #DOCKER_USERNAME and DOCKER_PASSWORD must be set on travis settings
       #if they are set, it will log in to docker and push to docker
       - if [[ ${DOCKERHUB} && ${DOCKER_USERNAME} && ${DOCKER_PASSWORD} && ${TRAVIS_BRANCH} == master && ${TRAVIS_PULL_REQUEST} == false ]]; then
           docker login -u="$DOCKER_USERNAME" -p="$DOCKER_PASSWORD";
           if [[ ${CC} == gcc ]]; then docker push "${TRAVIS_REPO_SLUG}:latest"; fi;
         fi
       #only runs on builds where COVERAGE=ON and DISTRO=fedora
       - if [[ ${DISTRO} = fedora && ${COVERAGE} && ${CC} = gcc ]]; then
           cd $HOME/docker/git-src;
           #copies encrypted deploy key
           cp deploy.enc $HOME;
           #move to gh-pages branch
           git fetch origin gh-pages && git checkout -b gh-pages FETCH_HEAD;
           docker cp ${CON}:/home/user/git-src/build/html . ;
           mv html/* .;
           rmdir html; 
           git add --all;
           #must be using a SSH deploy key--replace value
           if [[ ${TRAVIS_BRANCH} = master && ${<encrypted_SSH_deploy_key>} && ${<encrypted_SSH_deploy_key>} && ${TRAVIS_PULL_REQUEST} == false ]]; then
             git config --global user.name "Automatic Deployment (Travis CI)";
             git config --global user.email "abc@abc.com";
             git commit -m "Documentation Update";
             openssl aes-256-cbc -K $<encrypted_SSH_deploy_key> -iv $<encrypted_SSH_deploy_key> -in $HOME/deploy.enc -out ~/.ssh/id_rsa -d;
             chmod 600 ~/.ssh/id_rsa;
             #push doxygen documentation
             git push git@github.com:${TRAVIS_REPO_SLUG} gh-pages:gh-pages;
           else
             git status;
             git diff --cached --no-color | head -n 500;
           fi;
         fi

     cache:
       ccache: true
       directories:
         - $HOME/.sonar

     compiler:
       - gcc
       - clang

In the DOXYFILE, change the name to DOXYFILE.in and `INPUT = @CMAKE_SOURCE_DIR@`, `GENERATE_LATEX = NO`, and `GENERATE_XML = YES`.

The CMakeLists.txt has a similar initial block and Doxygen block, however, now CMake also does a coverage build 

cmake_minimum_required (VERSION 2.6)
project (helloworld)
add_executable(helloworld helloworld.c)
install(TARGETS helloworld DESTINATION bin)

option(ENABLE_COVERAGE_BUILD "Do a coverage build" ON)
if(ENABLE_COVERAGE_BUILD)
  message(STATUS "Enabling coverage build")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} --coverage -O0")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} --coverage -O0")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} --coverage")
  set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} --coverage")
endif()

find_package(Doxygen)
if (DOXYGEN_FOUND)
  configure_file(DOXYFILE.in ${CMAKE_CURRENT_BINARY_DIR}/DOXYFILE @ONLY)
  add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/html/index.html
    COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/DOXYFILE
    DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/DOXYFILE
    COMMENT "Build doxygen documentation")
  add_custom_target(html DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/html/index.html)
endif()

enable_testing()
add_test (TestRun helloworld)

option(ENABLE_COVERAGE_BUILD "Do a coverage build" OFF)
if(ENABLE_COVERAGE_BUILD)
  message(STATUS "Enabling coverage build")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} --coverage -O0")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} --coverage -O0")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} --coverage")
  set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} --coverage")
endif()
