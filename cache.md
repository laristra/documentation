## Cache
Caches store data to speed up processes, for example, requests are temporarily stored so that the same request later could be served faster. Travis CI [caches dependencies and directories](https://docs.travis-ci.com/user/caching/) which makes the  build go quicker. To enable [ccache](https://ccache.samba.org/), add the following lines to the .travis.yml file.

    #install ccache in before_install, then run it if the build passes

    before_install:
      - ccache -z
      
    after_success:
      - ccache -s
      
    cache:
      ccache: true
