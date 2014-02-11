cf-node-hello-git
================================================================================

An example of pushing a git repo of a node.js app to CloudFoundry, using
[Jame's Bayer's scm-buildpack](https://groups.google.com/a/cloudfoundry.org/d/msg/vcap-dev/9eC_wNg2DPc/FcYDlrkeH7UJ).


usage
================================================================================

run:

    git clone https://github.com/pmuellr/cf-node-hello-git
    cd cf-node-hello-git
    <edit manifest.yml, change application `name` value>
    cf push

and behold your new hello world server is running!



references
================================================================================

* <https://github.com/pmuellr/cf-node-hello.git> :
  git repo containing the app we want to push

* <https://github.com/ddollar/heroku-buildpack-multi.git> :
  git repo containing the multi-buildpack buildpack

* <https://github.com/jbayer/scm-buildpack.git> :
  git repo containing the git buildpack

* <https://github.com/cloudfoundry/heroku-buildpack-nodejs.git> :
  git repo containing the node buildpack



files
================================================================================



`.buildpacks`
--------------------------------------------------------------------------------

Contains two entries,

    https://github.com/jbayer/scm-buildpack.git
    https://github.com/cloudfoundry/heroku-buildpack-nodejs.git

The first is Jame's Bayer's awesome
[scm-buildpack](https://github.com/jbayer/scm-buildpack)
which does all the git cloning work on the staging server.

The second is the actual buildpack we want to use with our the app we want
pushed to CloudFoundry; it's a node app, so we're using the default node
buildpack.



`.scmbuildpack`
--------------------------------------------------------------------------------

This is a just a marker file, it can be empty.  The `scm-buildpack` buildpack
checks for this file's existance before continuing.



`manifest.yml`
--------------------------------------------------------------------------------

This is the manifest for your application.

    applications:
    - name: cf-node-hello-blortz
      memory: 128M
      buildpack: https://github.com/ddollar/heroku-buildpack-multi.git
      path: .
      env:
         SCM_URL: https://github.com/pmuellr/cf-node-hello.git

Some notes:

* `name` is the application name that's pushed.  **CHANGE IT!!**

* `buildpack` points to a special buildpack that runs multiple buildpacks; in
  this case, the buildpacks specified in the `.buildpacks` file.

* `SCM_URL` should be set to the git repo you want to pull from; this is the git
  repo of the actual app

* The "start command" for this app is specified in the
  [`Procfile`](https://github.com/pmuellr/cf-node-hello/blob/master/Procfile)
  of the app itself.



`README.md`
--------------------------------------------------------------------------------

This file!

