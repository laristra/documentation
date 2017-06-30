# How to
How to use GitHub, Travis CI, CodeCov, SonarQube, Doxygen, Make, CMake, and/or Doxygen using c or c++ with [c repository](https://github.com/laurelmcintyre/c) as example -- refer to [documentation_python.md](https://github.com/laurelmcintyre/documentation/blob/gh-pages/instructions_python.md) for more detailed instructions

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

       script: gcc <file-name>.c -o <file-name>

       compiler:
         - gcc
         - clang
    * Go to Travis and switch the tab from off to on on the new repo. The first build should start eventually.

6. Put Travis Badge on GitHub
    * Next to the repo name on Travis should be a build badge. Click on it and copy the link in Markdown format to paste on the repo's README page.

7. Make a Deploy Key for the Repo
In place of a personal access token which can be used to access all of an organization's repos, a deploy key/SSH key is specific to a repo and therefore is safer. To create a deploy key, go to Terminal. 

    * `git pull` to make sure the repo is up to date on the local machine
    * `ssh keygen -f deploy` to generate a deploy key
    * Go to GitHub settings and click Add Deploy Key
    * `cat deploy.pub` .pub indicates the public key and cat displays the content of the public deploy key in terminal
    * copy the content into the window on GitHub that "Add Deploy Key" opens and click "Allow write access"
    Now, you will add the private key, encrypted, to GitHub, which travis commands in Terminal automatically generate
    * `travis encrypt-file ./deploy` tells travis to encrypt the private deploy key
    * `le -add after success` tells to add in the after_success portion of the .travis.yml file
    * type yes when prompted
    * `git add deploy.enc` .enc implies the encrypted version of the deploy key, add this to github
    * `git add .travis.yml` the .travis.yml file now has the encrypted key on it, so add this to github as well
    * `git commit -m "encrypt deploy key"
    * Edit the .travis.yml file in terminal and add:
    
          mv deploy ~/.ssh/id-rsa

          chmod 600 ~/.ssh/id-rsa
       
      The first command means the deploy key will be in the ssh folder and the second chmod 600 command means that only you have read/write access and no one else. The chmod command is in the XYZ format, with X meaning you, Y meaning your group, and Z meaning everyone else. The numbers range from 0 - 7 and have different meanings, such as 2 means write and 4 means read, so 6 = 2 + 4 and has both these permissions.

8. CodeCov 
    * Add to .travis.yml file

            before_install:
              - ccache -z

            script:
              - ${CC} --coverage -c <file_name>.c -o <file_name>.o
              - ${CC} --coverage <file_name>.o -o <file_name>
              - ./<file_name>
            after_success:
              - ccache -s
              - if [ ${CC} = clang ]; then
                  bash <(curl -s https://codecov.io/bash) -F ${CC} --gcov-exec "llvm-cov gcov";
                else
                  bash <(curl -s https://codecov.io/bash) -F ${CC};
                fi

            cache:
              ccache: true
    * Create .codecov.yml file
    
            coverage:

              precision: 1

              round: down

            range: "70...100"

    * Go to [the CodeCov website](http://codecov.io/), log in, and add the new repo -- then the codecoverage dashboard should appear
    * Go to settings and copy the markdown version of the badge to post on the README file
    
9. SonarQube
    * Log in to SonarQube and generate a new token under account settings -- put the token called SONAR_TOKEN in environmental variables on Travis
    * Add to .travis.yml file
    
            addons:
              sonarcloud:
                organization: "<username>-github"


            script:
              - sonar-scanner

            cache:
              ccache: true
              directories:
                - $HOME/.sonar
    * Make a sonar-project.properties file
    
            sonar.projectKey=<project_name>:master

            sonar.projectName=<project-name>


            sonar.projectVersion=1.0

            # sonar.cfamily.build-wrapper-output.bypass=true

            sonar.sources=.


            sonar.sourceEncoding=UTF-8
   
   * Display quality gate badge on README - format: `[![Quality Gate](https://sonarqube.com/api/badges/gate?key=<repo_name>%3Amaster)](https://sonarqube.com/dashboard?id=<repo_name>%3Amaster)`

              
10. Doxygen 
    * Download Doxygen to your computer, go to a directory, and run `doxygen -g`. This will create a Doxyfile. Then `open Doxyfile` to get the template and standard settings for a Doxyfile. Put this in your GitHub repo as Doxyfile.
    * Create a gh-pages branch
    * Create generateDocumentationAndDeploy.sh file
    
            echo 'Setting up the script...'


            set -e

            
            mkdir code_docs
            
            cd code_docs

            
            git clone -b gh-pages https://github.com/<username>/<repo_name> <repo_name>
            
            cd <repo_name>


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


                git push --force "https://${GH_REPO_TOKEN}@${GH_REPO_REF}" > /dev/null 2>&1
            else
                
                echo '' >&2
                
                echo 'Warning: No documentation (html) files have been found!' >&2
                
                echo 'Warning: Not going to push the documentation to GitHub!' >&2
                    
                exit 1
            fi
            
    * Next, add to your .travis.yml file:

            branches:
              except:
                - gh-pages

            env:
              global:
                - GH_REPO_NAME: <repo_name>
                - DOXYFILE: $TRAVIS_BUILD_DIR/DOXYFILE
                - GH_REPO_REF: github.com/<username>/<repo_name>.git

            addons:
              sonarcloud:
                organization: "<username>-github"
              apt:
                packages:
                  - doxygen
                  - doxygen-doc
                  - doxygen-latex
                  - doxygen-gui
                  - graphviz

            after_success:
              - cd $TRAVIS_BUILD_DIR
              - chmod +x generateDocumentationAndDeploy.sh
              - ./generateDocumentationAndDeploy.sh
   
    * Doxygen documentation should be displayed on the gh-pages branch. Go into settings to find the link where it is published, which should say "Your site is published at https://<username>.github.io/<repo_name>/". Add html after <repo_name>/ to see the published documentation.

11. Makefile -- new example repo : [c-makefile](https://github.com/laurelmcintyre/c-makefile)
    * Create MakeFile in repo with following lines (simple example):
    
            all: helloworld.c hello.c ; gcc hello.c -o hello -g ; gcc helloworld.c -o helloworld -g

            clean: ; rm hello ; rm helloworld
            
    * Common build failure such as "Makefile:2: missing separator.  Stop." is caused by indentation errors and can be fixed with semicolon format as shown above
    * Add `script : -make` to .travis.yml file
    
12. CMake -- new example repo : [cpp-cmake](https://github.com/laurelmcintyre/cpp-cmake)
    * Create CMakeLists.txt :
    
          cmake_minimum_required (VERSION 2.6)

          project (<repo_name>)

          add_executable(<repo-name> <file_name>)


          set_property(TARGET <repo_name> PROPERTY CXX_STANDARD 11)

          set_property(TARGET <repo_name> PROPERTY CXX_STANDARD_REQUIRED ON)
 
    * Add to .travis.yml file:
    
          addons:
            apt:
              packages:
                - cmake-data
                - cmake
                

          script:
            - g++ -coverage <file_name>.cpp -o <file_name>
            - cmake CMakeLists.txt
            - make
            - ./<repo_name>
            
          compiler:
            - g++
            
