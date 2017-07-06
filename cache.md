## Cache
Caches store data to speed up processes, for example, requests are temporarily stored so that the same request later could be served faster. Travis CI [caches dependencies and directories](https://docs.travis-ci.com/user/caching/) which makes the  build go quicker. To enable a compiler cache, in this case [ccache](https://ccache.samba.org/), you need to add `ccache: true` under `cache`, but the before_install and after_success blocks are optional. The before_install block (-z tag) tells ccache to reset the statistics. The after_success block (-s tag) records new statistics if the build passes.
  
    before_install:
      - ccache -z
      
    after_success:
      - ccache -s
      
    cache:
      ccache: true
