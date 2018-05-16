# JEC ClassLoader

The JEC [`ClassLoader`](http://jecproject.org/docs/jec-commons/api/interfaces/classloader.html) interface is a part of the JEC specification that allows to dynamically loads JEC classes into the JavaScript Container.

The [`ClassLoader`](http://jecproject.org/docs/jec-commons/api/interfaces/classloader.html) is the part of the JEC Environment that loads classes into memory, only when they are needed.

## Synchronism

The JEC specification defines that class loading process is synchronous. The reason is that JavaScript Entreprise Containers mostly use ClassLoaders at server starting time. During this phase, containers must ensure that class dependencies are correctly solved to prevent server failures.

Developers can access to built-in ClassLoaders to create their own JEC APIs implementations. If you need an asynchronous loading process, you have to create your own class loading engine.

## Working with the default JEC ClassLoader

### ClassLoader context