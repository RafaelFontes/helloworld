# NodeJS

## What is Node.js?

* JavaScript runtime built on Chrome's V8 JavaScript engine
* JavaScript running on the server
* Works on a single thread using non-blocking I/O calls

> [Download](https://nodejs.org/en/download/)

#NPM

## What is NPM?

* JavaScript package manager ( similar to composer, apt-get, yum, etc... )
* Used to install and/or publish programs/modules
* It is installed together with [nodejs installer](https://nodejs.org/en/download/)


### Usage:
```
npm <command>

where <command> is one of:
    access, adduser, bin, bugs, c, cache, completion, config,
    ddp, dedupe, deprecate, dist-tag, docs, edit, explore, get,
    help, help-search, i, init, install, install-test, it, link,
    list, ln, login, logout, ls, outdated, owner, pack, ping,
    prefix, prune, publish, rb, rebuild, repo, restart, root,
    run, run-script, s, se, search, set, shrinkwrap, star,
    stars, start, stop, t, tag, team, test, tst, un, uninstall,
    unpublish, unstar, up, update, v, version, view, whoami

npm <cmd> -h     quick help on <cmd>
npm -l           display full usage info
npm help <term>  search for help on <term>
npm help npm     involved overview

```

Configuration of npm is made thru a file named package.json

example of a package.json file:

```
{
  "name": "name-of-a-module",
  "version": "1.0.0",
  "description": "Hello World Module",
  "scripts": {
    "hello-world": "echo Hello World"
  },
  "author": "Rafael",
  "dependencies": {
  }
}
```

## How to start a project with npm?

![npm-init.gif](https://github.com/poste9/helloworld/raw/master/recs/npm-init.gif)

In my case I have to press Ctrl+C after finishing to stop the process.
But what is really important is that package.json file is created.

```
{
  "name": "hello-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
````

being on the path of a package.json file, one can start to use npm to manage dependencies of your project

```
$/c/dev/github/workspace> npm install mocha --save-dev
``` 

This will install [Mocha](https://github.com/poste9/helloworld/blob/master/mocha) to your project.

### What do you mean by install?

Well.. when you use the flag ***--save-dev*** or ***--save*** it will download and save under the folder node_modules of your project what you provided if available, as well as its dependencies (most of the time)

Also, it will change your package.json file with the information of that dependency

```
{
  "name": "hello-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "mocha": "^3.2.0"
  }
}
```

### What is that _^_ after the version of the module?

By default npm takes the actual version of the module and place on your package.json to make sure when someone uses your package.json, that person will take at least the same version you did. But because of the _^_ thing, if there is a new version available, npm will download the new one.

### How to prevent npm to place the _^_ thing ?

```
$/c/dev/github/workspace> npm install chai --save-dev --save-exact
```

Now the package.json will have another dependency on the list

```
...
  "devDependencies": {
    "chai": "3.5.0",
    "mocha": "^3.2.0"
  }
}
```

Of course you can always open the package.json file with your favorite text editor and replace anything you want.

### How to run scripts with npm ?

First lets change our package.json file a bit

```
{
  "name": "hello-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "custom-script": "echo \"It's just a custom script\""
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "chai": "3.5.0",
    "mocha": "3.2.0"
  }
}
```

We have replace the script "test" and to the one called "custom-script".

To run the script you just need to type:

```
$/c/dev/github/workspace> npm run custom-script
```

Only thing left to say is when you run a script, you can use any program installed globally or locally by npm.

For example, lets add another script:

```
...
  "scripts": {
    "custom-script": "echo \"It's just a custom script\"",
    "test" : "mocha" 
  },
...
```

Now, by running the _test_  command npm will run mocha, even if you don't have mocha installed globally