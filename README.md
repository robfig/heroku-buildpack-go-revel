# Heroku Buildpack: Revel

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks)
for [Revel](http://robfig.github.io/revel/).

## Heroku-specific build tag

The buildpack adds a `heroku` [build constraint][build-constraint],
to enable heroku-specific code. See the [App Engine build constraints article][app-engine-build-constraints]
for more.

## Usage

### Setup

The buildpack requires a `.godir` file in the project root directory to tell it
the import path to your Revel application.  The contents of `.godir` should be
exactly the argument to "revel run" when running the application.

Here is an example session.

```
$ pwd
/Users/robfig/gocode/src/github.com/robfig/helloworld

$ echo "github.com/robfig/helloworld" > .godir

$ find . -type f
./.godir
./app/controllers/app.go
./app/views/Application/Index.html
./conf/app.conf
./conf/routes

$ heroku create -b https://github.com/revel/heroku-buildpack-go-revel.git
...
```

### Deployment

Once the `.godir` and heroku remote are set up, deployment is a single command.

```
$ git push heroku master
...
-----> Fetching custom git buildpack... done
-----> Revel app detected
-----> Installing Go 1.1... done
       Installing Virtualenv... done
       Installing Mercurial... done
       Installing Bazaar... done
-----> Running: go get -tags heroku ./...
-----> Discovering process types
       Procfile declares types -> (none)
       Default types for Revel -> web

-----> Compiled slug size: 33.3MB
-----> Launching... done, v5
       http://pure-sunrise-3607.herokuapp.com deployed to Heroku
```

The buildpack will detect your repository as Revel if it
contains the `conf/app.conf` and `conf/routes` files.

It's possible to specify the revel mode by setting the REVEL_MODE environment variable. "prod" will be the default mode.

#### Dependencies and private repositories
If you want to use private repositories you just need to create Godeps folder with this command:
```
$ godep save ./...
```
and commit your changes:
```
$ git add -A . 
$ git commit -a -m "Dependencies"
```
Once the `Godeps` created and committed you can push your changes (deploy):
```
$ git push heroku master
```
## Hacking on this Buildpack

To change this buildpack, fork it on GitHub. Push
changes to your fork, then create a test app with
`--buildpack YOUR_GITHUB_GIT_URL` and push to it. If you
already have an existing app you may use `heroku config:add
BUILDPACK_URL=YOUR_GITHUB_GIT_URL` instead of `--buildpack`.

[go]: http://golang.org/
[buildpack]: http://devcenter.heroku.com/articles/buildpacks
[quickstart]: http://mmcgrana.github.com/2012/09/getting-started-with-go-on-heroku.html
[build-constraint]: http://golang.org/pkg/go/build/
[app-engine-build-constraints]: http://blog.golang.org/2013/01/the-app-engine-sdk-and-workspaces-gopath.html

