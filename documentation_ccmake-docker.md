# How to
How to use GitHub, Travis CI, CodeCov, SonarQube, Doxygen, CMake, and Docker using c with [example ccmake-docker repository](https://github.com/laurelmcintyre/ccmake-docker) 

This repo goes through the same initial steps as the cpp-cmake repo. If you want to create the repo with Docker from the start, read the Docker instructions (bottom of this file) first.

## GitHub
[How to set up GitHub account, create SSH key for GitHub, and clone repository to local computer](https://github.com/laurelmcintyre/documentation/blob/gh-pages/github_instructions.md)

### The Shell and Terminal
[Shell/Terminal description and Terminal commands](https://github.com/laurelmcintyre/documentation/blob/gh-pages/shell_and_terminal.md)

### Markdown
[Markdown formatting description](https://github.com/laurelmcintyre/documentation/blob/gh-pages/markdown.md)

## Travis CI
[Continuous Integration description, Travis CI setup, build passing badge](https://github.com/laurelmcintyre/documentation/blob/gh-pages/travis_ci.md)

### Example of a .travis.yml file
[Link to example ccmake-docker repository](https://github.com/laurelmcintyre/ccmake-docker)
* `language: c` means that the project is written in c.
* `script: ${CC} <file-name>.c -o <file-name>` runs the compiler gcc or clang on a file
*  Specify the compiler which the script runs on

        compiler:
          - gcc
          - clang 

## Create a Personal Access Token OR Create a Deploy Key for the Repo
[Create personal access token](https://github.com/laurelmcintyre/documentation/blob/gh-pages/gh_personal_access_token.md) OR [Create an SSH deploy key](https://github.com/laurelmcintyre/documentation/blob/gh-pages/deploy_key.md) -- SSH deploy key is more secure

## Cache
[Cache description, additions to .travis.yml file](https://github.com/laurelmcintyre/documentation/blob/gh-pages/cache.md)
      
## Code Coverage
[CodeCov, .codecov.yml file, and code coverage badge](https://github.com/laurelmcintyre/documentation/blob/gh-pages/codecov.md)

### Additions to .travis.yml file needed for Code Coverage
    #sudo:required and dist:trusty both specify a trusty build environment
    sudo: required

    dist: trusty
    
    #${CC} is replaced by gcc and clang cmpilers
    #runs coverage on files
    script:
      - ${CC} --coverage -c <file_name>.c -o <file_name>.o
      - ${CC} --coverage <file_name>.o -o <file_name>
      - ./<file_name>
    
    #runs if the build passes
    #if the compiler is clang, it needs llvm-cov instead of gcov
    after_success:
      - if [ ${CC} = clang ]; then
          bash <(curl -s https://codecov.io/bash) -F ${CC} --gcov-exec "llvm-cov gcov";
        else
          bash <(curl -s https://codecov.io/bash) -F ${CC};
        fi
                 
## SonarQube
[SonarCloud, sonar-project.properties file, quality gate badge](https://github.com/laurelmcintyre/documentation/blob/gh-pages/sonarcloud.md)

You will need to add these lines to your .travis.yml file:

      addons:
        sonarcloud:
          organization: "<username>-github"

      script:
        - sonar-scanner

      cache:
        ccache: true
        directories:
          - $HOME/.sonar
          
## Doxygen
[How to set up Doxygen, generateDocumentationAndDeploy.sh file, additions to .travis.yml file](https://github.com/laurelmcintyre/documentation/blob/gh-pages/doxygen.md)

## CMake
[How to set up CMakeLists.txt file and .travis.yml file](https://github.com/laurelmcintyre/documentation/blob/gh-pages/cmake.md)

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

    #alobbs/doxy-coverage is a repo
    #in the DOXYFILE, must be GENERATE_XML = ON
    
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

In the repo, delete the `sonar-project.properties file` and the `generateDocumentationAndDeploy.sh` file. The  `.travis.yml` file, `CMakeLists.txt` file, and `DOXYFILE` will be edited, and you will make a `Dockerfile`. Much of the travis commands are placed instead in the Dockerfile. Therefore, the commands are all in the same place and now can be built from any machine because of Docker. Travis variables/environmental variables are also transferred to Docker. Four environmental variables will need to be set in Travis or else the build will fail because the variables will not exist: DOCKER_USERNAME, DOCKER_PASSWORD, SONARQUBE_GITHUB_TOKEN (github repo token), and SONARQUBE_TOKEN (instructions above under SonarQube on how to generate token in SonarCloud). This example also uses a SSH deploy key, which is automatically put under Travis environmental variables. The other variables below, i.e. ${CC} or ${TRAVIS_JOB_NUMBER}, are autogenerated by Travis.
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
      #copy caches from travis to Docker
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
      #cache for sonarqube
        - $HOME/.sonar

    compiler:
      - gcc
      - clang

In the DOXYFILE, change the name to DOXYFILE.in and the settings to `INPUT = @CMAKE_SOURCE_DIR@`, `GENERATE_LATEX = NO`, and `GENERATE_XML = YES`.

The CMakeLists.txt has a similar initial block and Doxygen block as before, however, now CMake also does a coverage build and adds a test for the helloworld file. The helloworld file is an example and should be replaced. 

    ccmake_minimum_required (VERSION 2.6)
    project (helloworld)
    add_executable(helloworld helloworld.c)
    install(TARGETS helloworld DESTINATION bin)

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
    
The Dockerfile (in a folder called docker) is passed variables/arguments from Travis and runs much of the same things, but in Docker instead of Travis.

    #pulls from buildenv repo
    FROM laurelmcintyre/buildenv:fedora

    #variables passed from travis in .travis.yml file
    ARG COVERAGE
    ARG SONARQUBE
    ARG SONARQUBE_TOKEN
    ARG SONARQUBE_GITHUB_TOKEN
    ARG CC
    ARG CXX

    #used by codecov.bash
    ARG CI
    ARG TRAVIS
    ARG TRAVIS_BRANCH
    ARG TRAVIS_JOB_NUMBER
    ARG TRAVIS_PULL_REQUEST 
    ARG TRAVIS_JOB_ID
    ARG TRAVIS_TAG
    ARG TRAVIS_REPO_SLUG
    ARG TRAVIS_COMMIT
    ARG TRAVIS_OS_NAME

    RUN rm -rf /home/user/.ccache
    COPY git-src /home/user/git-src
    #copy caches from docker to docker image
    COPY ccache/ /home/user/.ccache
    COPY sonar/ /home/user/.sonar
    USER root
    #chown -- change owner from root to user
    RUN chown -R user:user /home/user/git-src /home/user/.ccache /home/user/.sonar
    USER user

    #navigate to git-src, then make directory called build
    WORKDIR /home/user/git-src
    RUN mkdir build
    
    #similar to script block in .travis.yml-- runs ccache, make, doxygen (html), coverage, sonarqube
    WORKDIR build
    RUN ccache -z
    RUN cmake ${COVERAGE:+-DENABLE_COVERAGE_BUILD=ON} ..
    RUN ${SONARQUBE:+build-wrapper-linux-x86-64 --out-dir bw-output} make VERBOSE=1 -j2
    RUN ccache -s
    RUN make test
    RUN make install DESTDIR=${PWD}
    RUN make html
    RUN cd .. && if [ ${COVERAGE} ]; then \
       python -m coverxygen --xml-dir build/xml/ --src-dir . --output doxygen.coverage.info; \
       wget -O codecov.sh https://codecov.io/bash; \
       bash codecov.sh -X gcov -f doxygen.coverage.info -F documentation; \
      if [ ${CC} = clang ]; then \
        $HOME/.local/bin/codecov -F ${CC} --gcov-exec "llvm-cov gcov"; \
      else \
        $HOME/.local/bin/codecov -F ${CC}; \
      fi; \
    fi && cd -
    RUN cd .. && if [ ${SONARQUBE} ]; then \
      [ ${COVERAGE} ] || find . -type f -name '*.gcno' -exec gcov -pb  {} +; \
      sonar-scanner \
        -Dsonar.projectKey=${TRAVIS_REPO_SLUG##*/} \
        -Dsonar.projectName=${TRAVIS_REPO_SLUG#*/} \
        -Dsonar.projectVersion=${TRAVIS_COMMIT} \
        -Dsonar.branch=/${TRAVIS_BRANCH} \
        -Dsonar.links.homepage=http://github.com/${TRAVIS_REPO_SLUG} \
        -Dsonar.links.ci=https://travis-ci.org/${TRAVIS_REPO_SLUG} \
        -Dsonar.links.scm=https://github.com/${TRAVIS_REPO_SLUG} \
        -Dsonar.links.issue=https://github.com/${TRAVIS_REPO_SLUG}/issues \
        -Dsonar.sources=. \
        -Dsonar.exclusions=build/CMakeFiles/**,build/html/**,build/xml/**,tests/** \
        -Dsonar.cfamily.build-wrapper-output=build/bw-output \
        -Dsonar.host.url=https://sonarqube.com \
        -Dsonar.cfamily.gcov.reportsPath=. \
        -Dsonar.organization=${TRAVIS_REPO_SLUG%%/*}-github \
        $([ ${TRAVIS_PULL_REQUEST} != false ] && echo \
           -Dsonar.analysis.mode=preview \
           -Dsonar.github.pullRequest=${TRAVIS_PULL_REQUEST} \
           -Dsonar.github.repository=${TRAVIS_REPO_SLUG} \
           -Dsonar.github.oauth=${SONARQUBE_GITHUB_TOKEN}) \
        -Dsonar.login=${SONARQUBE_TOKEN}; \
    fi && cd -
    USER root
    RUN make install
    USER user
    WORKDIR /home/user
