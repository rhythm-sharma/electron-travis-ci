# It is an Eample for creating electron artifacts using Travis-Ci for linux and Macos 

#### explaination
I have created this project as an example for building electron artifacts using Travis-Ci for linux and Macos. I've used electron-builder for building installers for all OS, Created .travis.xml and added build and script section in package.json  

#### Steps
* Create an electron project and push to github
* if you are new to setup travis-ci for github repository then check out this article here [continuous-integration-using-travis-on-github](https://hackernoon.com/continuous-integration-using-travis-on-github-1f7f2314b6b7)
* create a `.travis.xml` file, which tells Travis-ci what to do?
* checkout this tutorial on how to write .travis.xml files? [travis-ci-tutorial](https://docs.travis-ci.com/user/tutorial/) and also checkout my [.travis.xml](https://github.com/rrhythmsharma/electron-travis-ci/blob/master/.travis.yml) file for reference
* It is important that you must add below code inside .travis.xml for building electron artifacts, checkout this [electron doc](https://electronjs.org/docs/tutorial/testing-on-headless-ci#travis-ci) for reason.
    * addons:
      apt:
        packages:
          - xvfb    
    * install:
      - npm install electron-builder@next
      - export DISPLAY=':99.0'
      - Xvfb :99 -screen 0 1024x768x24 > /dev/null 2>&1 &

    * before_script:
      - export DISPLAY=:99.0
      - sh -e /etc/init.d/xvfb start &
      - sleep 3

* now configure package.json by adding build script for linux and macOS
* I've added two scripts, first one is dist:linux(for linux) and second one is dist:osx(for macOS)
* now add build property in package.json which defines your build targets, checkout my [package.json#L15](https://github.com/rrhythmsharma/electron-travis-ci/blob/master/package.json#L15) for reference.
* I've added "deb", "rpm", "AppImage", "zip" inside target for linux then defined the targets and for macOS added only "dmg" then defined the dmg target.
* I've added the deploy section for automatic deployment of artifacts, which uses your github_token for more info check this [github-auth-token-on-travis](https://blog.wyrihaximus.net/2015/09/github-auth-token-on-travis/) out 
* that's it, push this changes to your github repository and see the result 
* if everything is right an automatic build will be started by travis, which first build the defined targets in package.json then deploy them to your github release page on every commit.

### create an issue if you are facing problems :)