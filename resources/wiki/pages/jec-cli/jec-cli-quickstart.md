# JEC CLI QuickStart

This small example shows you how to create a new [GlassCat Server](./docs/reference/glasscat/glasscat-server) instance. You need to have a functional [Node.js](https://nodejs.org/) environment installed.

**We assume that you are familiar with Node.js and npm.**

## Installation

Set up the JEC CLI module with:

```bash
$ [sudo] npm install jec-cli -g
```
Check whether JEC CLI has been installed successfully:

```bash
$ jec version
```

JEC CLI is now available everywhere from your development environment.

## Create a new GlassCat server

Create a new empty folder and move into it:

```bash
$ mkdir glasscat-test
$ cd glasscat-test
```

Run the install command:

```bash
$ jec glasscat-install
```

At the end of the install process, you should see the following logs:

```bash
      _   _____    ____      ____   _       ___
     | | | ____|  / ___|    / ___| | |     |_ _|
  _  | | |  _|   | |       | |     | |      | |
 | |_| | | |___  | |___    | |___  | |___   | |
  \___/  |_____|  \____|    \____| |_____| |___|

Installing GlassCat 0.1.4.
  download https://registry.npmjs.org/jec-glasscat/-/jec-glasscat-0.1.4.tgz
  extract jec-glasscat-0.1.4.tgz
  remove C:\Users\Desktop\jec-glasscat
Installing packages for tooling via npm.
  compile compiling TypeScript files
  build creating server directories
  build copying config files
  build copying core directories
  build installing packages for admin console via npm
  build installed packages for admin console via npm
Installed packages for tooling via npm.
Removing temporary files.
  remove C:\Users\Desktop\glasscat-test\tsconfig.json
  remove C:\Users\Desktop\glasscat-test\package.json
  remove C:\Users\Desktop\glasscat-test\Gruntfile.js
  remove C:\Users\Desktop\glasscat-test\.npmignore
  remove C:\Users\Desktop\glasscat-test\.editorconfig
  remove C:\Users\Desktop\glasscat-test\juta
  remove C:\Users\Desktop\glasscat-test\test
  remove C:\Users\Desktop\glasscat-test\src
  remove C:\Users\Desktop\glasscat-test\.tscache
Server successfully installed.
```

Then, you can run the server in DEV mode:

```bash
$ glasscat start
```

Your server is now ready for e-business!
