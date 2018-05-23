# Welcome to Gofore's Angular Training!
## Setup
### Node.js, npm & git

Links for installers/installation instruction:
- [Node.js (newer than 8.9) & npm (newer than 5.0)](https://nodejs.org/)
- [git](http://git-scm.com/)

Please, make sure that the applications are installed correctly by checking their version numbers (which do not need to exactly match). Example of a valid setup:

```shell
> node --version
v8.9.0

> npm --version
v5.7.1

> git --version
git version 2.6.4
```

### Editor
You may use any IDE you feel comfortable with but we recommend __IntelliJ IDEA Ultimate__ (or WebStorm Ultimate) or __Visual Studio Code__ to be used since they provide very good support for Angular development.

If you prefer using Atom or Brackets, please, find instructions below.

#### IntelliJ IDEA Ultimate (or WebStorm)
- Proprietary (though 30-day trial available)
- Install recommended plugins through IntelliJ IDEA plugin management:
  - [_AngularJS_](https://github.com/JetBrains/intellij-plugins/tree/master/AngularJS)
  - [_NodeJS_](https://plugins.jetbrains.com/plugin/6098?pr=idea)

#### Visual Studio Code
- Free & open source
- Install recommended extensions:
  - [_TSLint_](https://marketplace.visualstudio.com/items?itemName=eg2.tslint)
  - [_Auto Import_](https://marketplace.visualstudio.com/items?itemName=steoates.autoimport)

### Google Chrome
[_Google Chrome_](https://www.google.com/chrome/) browser is needed for running and debugging Karma tests.

### Project Skeleton for Exercises
Go to your workspace directory (`Documents/GitHub` etc.) and run

```shell
npm install -g @angular/cli@6.0.3
ng new angular-training
cd angular-training
ng serve
```

to generate a new project and to start the server. Server is running okay when it says `webpack: bundle is now valid` on the last line. Then, visit `http://localhost:4200/` to check that _app works!_ is printed. Now your environment is ready for training. See you soon!
