# Welcome to Gofore's Angular 2 Training!
## Setup
### Node.js, npm & git

Links for installers/installation instruction:
- [Node.js version 6.x & npm version 3.x](https://nodejs.org/)
- [git](http://git-scm.com/)

Please, make sure that the applications are installed correctly by checking their version numbers (which do not need to exactly match). Example of a valid setup:

```shell
> node --version
v6.5.0

> npm --version
v3.10.3

> git --version
git version 2.6.4
```

### Editor
You may use any IDE you feel comfortable with but we recommend __IntelliJ IDEA Ultimate__ (or WebStorm Ultimate) or __Visual Studio Code__ to be used since they provide very good support for Angular 2 development.

If you prefer using Atom or Brackets, please, find instructions below.

#### IntelliJ IDEA Ultimate
- Proprietary (though 30-day trial available)
- Install recommended plugins through IntelliJ IDEA plugin management:
  - [_AngularJS_](https://github.com/JetBrains/intellij-plugins/tree/master/AngularJS)
  - [_NodeJS_](https://plugins.jetbrains.com/plugin/6098?pr=idea)

#### Visual Studio Code
- Free & open source
- Install recommended extentions:
  - [_TSLint_](https://marketplace.visualstudio.com/items?itemName=eg2.tslint)
  - [_Auto Import_](https://marketplace.visualstudio.com/items?itemName=steoates.autoimport)

#### Atom
- Free & lightweight
- TypeScript package (called [_atom-typescript_](https://atom.io/packages/atom-typescript)) needs to be installed
- No auto-import support

### Brackets
- Free, recommended version: 1.7
- Required extensions for TypeScript support (follow GitHub page installation instructions):
  - [_brackets-npm-registry_](https://github.com/zaggino/brackets-npm-registry)
  - [_brackets-typescript_](https://github.com/zaggino/brackets-typescript)
- No auto-import support

### Google Chrome
[_Google Chrome_](https://www.google.com/chrome/) browser is needed for running and debugging Karma tests.

### Project Skeleton for Exercises
Go to your workspace directory (`Documents/GitHub` etc.) and run

```shell
npm install -g angular-cli@1.0.0-beta.17
ng new angular2-training
cd angular2-training
ng serve
```

to generate a new project and to start the server. Server is running okay when it says `webpack: bundle is now valid` on the last line. Then, visit `http://localhost:4200/` to check that _app works!_ is printed. Now your environment is ready for training. See you soon!
