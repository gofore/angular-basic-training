# Welcome to Gofore's Angular 2 Training!
## Setup
### Node.js, npm & git

Links for installers/installation instruction:
- [Node.js (at least version 4 or 5) & npm](https://nodejs.org/)
- [git](http://git-scm.com/)

Please, make sure that the applications are installed correctly. Example of a valid setup:

```shell
> node --version
v4.4.3

> npm --version
v3.5.4

> git --version
git version 2.6.4
```

### Editor

You may use any IDE you feel comfortable with but as we can't master all of them, we recommend either Atom or IntelliJ IDEA (or WebStorm) to be used.

#### Atom
- Free & lightweight
- Easy to use
- TypeScript package (called [_atom-typescript_](https://atom.io/packages/atom-typescript)) needs to be installed

#### IntelliJ IDEA
- Proprietary (though 30-day trial available)
- Install recommended plugins through IntelliJ IDEA plugin management:
  - [_AngularJS_](https://github.com/JetBrains/intellij-plugins/tree/master/AngularJS)
  - [_NodeJS_](https://plugins.jetbrains.com/plugin/6098?pr=idea)

### Copy of _angular2-seed_ Project
Go to your workspace directory (`Documents/GitHub` etc.) and run

```shell
git clone https://github.com/angular/angular2-seed.git
```

to download the code and

```shell
npm install -g webpack webpack-dev-server typings typescript
npm install
npm start
```

to install the dependencies and start the server. Then, visit `http://localhost:3000/` to check everything is working correctly.
