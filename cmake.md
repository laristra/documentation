## CMake
[CMake](https://cmake.org/overview/) is a system that manages builds. To enable CMake, you will need a CMakeLists.txt file and edits to your .travis.yml file.

CMakeLists.txt file: 

    #defines the project, executable, and target
    cmake_minimum_required (VERSION 2.6)
    project (<repo_name>)
    add_executable(<repo_name> <file_name>.cpp)

    set_property(TARGET <repo_name> PROPERTY CXX_STANDARD 11)
    set_property(TARGET <repo_name> PROPERTY CXX_STANDARD_REQUIRED ON)
      
    #add Doxygen commands to the CMakeLists.txt file instead of the .travis.yml file
    find_package(Doxygen)
    if (DOXYGEN_FOUND)
      add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/html/index.html
        COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/DOXYFILE
        DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/DOXYFILE
        COMMENT "Build doxygen documentation")
      add_custom_target(html DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/html/index.html)
    endif()
      
    option(ENABLE_COVERAGE_BUILD "Do a coverage build" OFF)
    if(ENABLE_COVERAGE_BUILD)
      message(STATUS "Enabling coverage build")
      set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} --coverage -O0")
      set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} --coverage -O0")
      set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} --coverage")
      set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} --coverage")
    endif()

    enable_testing()
    add_test (TestRun cpp-cmake)
    
Finally, to configure the .travis.yml file correctly, add this to the .travis.yml file and replace the existing script with the following script (leaving sonar-scanner in place).

    #install cmake packages
    addons:
      apt:
        packages:
          - cmake-data
          - cmake

    script:
      - cmake . -DENABLE_COVERAGE_BUILD=ON
      - make
      #run tests
      - make test
      #run make on doxygen (html)
      - make html

The final .travis.yml file for CMake will look like this:

    language: cpp

    sudo: required

    dist: trusty

    branches:
      except:
        - gh-pages

    env:
      global:
        - DOXYFILE: $TRAVIS_BUILD_DIR/DOXYFILE

    addons:
      sonarcloud:
        organization: <username>-github
      apt:
        packages:
          - doxygen
          - doxygen-doc
          - doxygen-latex
          - doxygen-gui
          - graphviz
          - cmake-data
          - cmake

    before_install:
      - ccache -z

    script:
      - sonar-scanner
      - cmake . -DENABLE_COVERAGE_BUILD=ON
      - make
      - make test
      - make html

    compiler:
      - g++

    after_success:
      - ccache -s
      - bash <(curl -s https://codecov.io/bash)
      - openssl aes-256-cbc -K $<encrypted_key> -iv $<encrypted_key> -in deploy.enc -out deploy -d
      - mv deploy ~/.ssh/id-rsa
      - chmod 600 ~/.ssh/id-rsa
      - chmod +x generateDocumentationAndDeploy.sh
      - if [[ ${TRAVIS_JOB_NUMBER} = *.1 ]]; then ./generateDocumentationAndDeploy.sh; fi

    cache:
      ccache: true
      directories:
    - $HOME/.sonar
