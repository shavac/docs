---
name: Bee usage
sort: 2
---

# Introduction to Bee
Bee is a project for developing Beego rapidly. With Bee we can create, auto compile and reload, develop, test, and deploy easily.

## Installing Bee
You can install Bee by this:

	go get github.com/beego/bee

## Bee commands
Type `bee` in command line. We can see these messages:

```
Bee is a tool for managing beego framework.

Usage:

	bee command [arguments]

The commands are:

    new         create an application base on beego framework
    run         run the app which can hot compile
    pack        compress an beego project
    api         create an api application base on beego framework
    router      auto-generate routers for the app controllers
    test        test the app
    bale        packs non-Go files to Go source files	
```	

### new
`new` can create a new web project. You can create a new Beego projec by typing `bee new project_name` under gopath. It will generate all the project folders and files:

```
bee new myproject
[INFO] Creating application...
/gopath/src/myproject/
/gopath/src/myproject/conf/
/gopath/src/myproject/controllers/
/gopath/src/myproject/models/
/gopath/src/myproject/static/
/gopath/src/myproject/static/js/
/gopath/src/myproject/static/css/
/gopath/src/myproject/static/img/
/gopath/src/myproject/views/
/gopath/src/myproject/conf/app.conf
/gopath/src/myproject/controllers/default.go
/gopath/src/myproject/views/index.tpl
/gopath/src/myproject/main.go
13-11-25 09:50:39 [SUCC] New application successfully created!
```

```
myproject
├── conf
│   └── app.conf
├── controllers
│   └── default.go
├── main.go
├── models
├── static
│   ├── css
│   ├── img
│   └── js
└── views
    └── index.tpl

8 directories, 4 files
```

### run
When we develop Go project ony problem is we need to compile our project manually. `bee run` command will supervise the file system of Beego project by inotify so that we can see the result directly after the modifications for project.

```
bee run
13-11-25 09:53:04 [INFO] Uses 'myproject' as 'appname'
13-11-25 09:53:04 [INFO] Initializing watcher...
13-11-25 09:53:04 [TRAC] Directory(/gopath/src/myproject/controllers)
13-11-25 09:53:04 [TRAC] Directory(/gopath/src/myproject/models)
13-11-25 09:53:04 [TRAC] Directory(/gopath/src/myproject)
13-11-25 09:53:04 [INFO] Start building...
13-11-25 09:53:16 [SUCC] Build was successful
13-11-25 09:53:16 [INFO] Restarting myproject ...
13-11-25 09:53:16 [INFO] ./myproject is running...
```
我们打开浏览器就可以看到效果`http://localhost:8080/`:

![](../images/beerun.png)

After modifing the `default.go` file in controllers folder, we can see
these output in command line.

```
13-11-25 10:11:20 [EVEN] "/gopath/src/myproject/controllers/default.go": DELETE|MODIFY
13-11-25 10:11:20 [INFO] Start building...
13-11-25 10:11:20 [SKIP] "/gopath/src/myproject/controllers/default.go": CREATE
13-11-25 10:11:23 [SKIP] "/gopath/src/myproject/controllers/default.go": MODIFY
13-11-25 10:11:23 [SUCC] Build was successful
13-11-25 10:11:23 [INFO] Restarting myproject ...
13-11-25 10:11:23 [INFO] ./myproject is running...
```
Refresh browser we can see the result of the modifications are already
there.

### api command
The `new` command is used for crating new Web applications. I usually use Beego for developing API applications. We can use `api` command to create API applications. Here the result of running `bee api project_name`:

```
bee api apiproject
create app folder: /gopath/src/apiproject
create conf: /gopath/src/apiproject/conf
create controllers: /gopath/src/apiproject/controllers
create models: /gopath/src/apiproject/models
create tests: /gopath/src/apiproject/tests
create conf app.conf: /gopath/src/apiproject/conf/app.conf
create controllers default.go: /gopath/src/apiproject/controllers/default.go
create tests default.go: /gopath/src/apiproject/tests/default_test.go
create models object.go: /gopath/src/apiproject/models/object.go
create main.go: /gopath/src/apiproject/main.go
```

Here is the project structure of this API application:

```
apiproject
├── conf
│   └── app.conf
├── controllers
│   └── default.go
├── main.go
├── models
│   └── object.go
└── tests
    └── default_test.go
```

Compare to Web application, the API application doesn't have static and
views folder but a test module for unit testing.

### test command
This is a wrapper for `go test`. It can run the test cases in test foler
of Beego project.

```
bee test apiproject
13-11-25 10:46:57 [INFO] Initializing watcher...
13-11-25 10:46:57 [TRAC] Directory(/gopath/src/apiproject/controllers)
13-11-25 10:46:57 [TRAC] Directory(/gopath/src/apiproject/models)
13-11-25 10:46:57 [TRAC] Directory(/gopath/src/apiproject)
13-11-25 10:46:57 [INFO] Start building...
13-11-25 10:46:58 [SUCC] Build was successful
13-11-25 10:46:58 [INFO] Restarting apiproject ...
13-11-25 10:46:58 [INFO] ./apiproject is running...
13-11-25 10:46:58 [INFO] Start testing...
13-11-25 10:46:59 [TRAC] ============== Test Begin ===================
PASS
ok  	apiproject/tests	0.100s
13-11-25 10:47:00 [TRAC] ============== Test End ===================
13-11-25 10:47:00 [SUCC] Test finish
```

### pack command
`pack` command is used to compress the project into a single file. Then we can deploy the project by uploading and extracting the zip file to the server.

```
bee pack
app path: /gopath/src/apiproject
GOOS darwin GOARCH amd64
build apiproject
build success
exclude prefix:
exclude suffix: .go:.DS_Store:.tmp
file write to `/gopath/src/apiproject/apiproject.tar.gz`
```

We can see compressed file in the project folder:

```
rwxr-xr-x  1 astaxie  staff  8995376 11 25 22:46 apiproject
-rw-r--r--  1 astaxie  staff  2240288 11 25 22:58 apiproject.tar.gz
drwxr-xr-x  3 astaxie  staff      102 11 25 22:31 conf
drwxr-xr-x  3 astaxie  staff      102 11 25 22:31 controllers
-rw-r--r--  1 astaxie  staff      509 11 25 22:31 main.go
drwxr-xr-x  3 astaxie  staff      102 11 25 22:31 models
drwxr-xr-x  3 astaxie  staff      102 11 25 22:31 tests
```

### router command
This command doesn't worked yet. In the future it will generate the routers by analyse the function in controllers.

### bale command
This command doesn't worked yet. In the future it will compress all the static files in to a single binary file. So we don't need to deploy the static files including js, css, images and views when deploy the project 
