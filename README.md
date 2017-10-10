# adapt-v4-prototype
Readme for testing prototype.


## Simple installation instructions
0. Use node 6+ (4 wont work)  
1. Open your terminal of choice and run:
```bash
npm install -g adaptv4-cli
```
2. Find a place to create a course and run:
```bash
adaptv4 create course
```
3. As the folder change seem is a bit broken at the moment because ``adaptv4-cli`` default repositories aren't setup properly. You will need to:
```bash
cd my-adapt-course
adaptv4 install
```
4. Build it:
```bash
grunt dev
```

## Details

### Not working
* grunt
  * ``theme`` and ``menu`` parameters 
  * dependency inclusion for ``includes`` and ``excludes``

### New bits
* bower
  * [``.bowerrc``](https://github.com/adaptlearning/adapt_framework/blob/prototype4/.bowerrc) file allows install from multiple bower repositories
  * there is a test v4 bower repository at [http://adapt-bower-repository-v4.herokuapp.com/](http://adapt-bower-repository-v4.herokuapp.com/packages/) with some new plugins in it, its source code is [here](https://github.com/oliverfoster/node-bower-server) and it's running from heroku.

* plugins
  * are now flat, no more ``components/``, ``extensions/``, ``menu/`` or ``theme/`` folders
  * [can have specified dependencies ](https://github.com/oliverfoster/adapt-contrib-navigation/blob/master/bower.json#L12)
  * have an extra plugin type of ``adapt-core`` to provide the first layer of the build process before ``adapt-component``, ``adapt-extension``, ``adapt-menu`` and ``adapt-theme``
  * have a [``config.js``](https://github.com/oliverfoster/adapt-contrib-boot/blob/master/config.js) for each plugin rather than a global one for adapt_framework. this is hopefully only a temporary measure to ensure all of the plugins are able to configure the compilation process appropriately

* grunt
  * [``grunt-file-order``](https://github.com/cgkineo/grunt-file-order) was created to sort files in the build process
  * grunt builds the flattened plugins in plugin-type-order (all ``adapt-core`` first, all ``adapt-theme`` last) and then in dependency-order (roots first, most dependant last)
  * the grunt config is somewhat simplified as all plugins are treated equally and there are no secondary rules for the core
  * grunt now rewrites the runtime requirejs dependency strings to accomodate the new layout. ``coreJS/`` will be rewritten into `adapt-contrib-core/js/`, etc. and warnings will shown in the console for renamed namespaces
  * has some extra files to manage the plugins, to correct namespaces and to load requirejs configurations

* core
  * is now 5 plugins [``adapt-contrib-boot``](https://github.com/oliverfoster/adapt-contrib-boot), [``adapt-contrib-core``](https://github.com/oliverfoster/adapt-contrib-core), [``adapt-contrib-drawer``](https://github.com/oliverfoster/adapt-contrib-drawer), [``adapt-contrib-navigation``](https://github.com/oliverfoster/adapt-contrib-navigation) and [``adapt-contrib-notify``](https://github.com/oliverfoster/adapt-contrib-notify). these 5 plugins are currently just an example of how the core can/should be sub-divided. i picked the easy bits.
  * these plugins are quite easy to override and extend now as they each have a defined scope.
  * the framework branch for this prototype is [prototype4](https://github.com/adaptlearning/adapt_framework/tree/prototype4), the ``src/core`` folder is removed as is the ``src/index.html`` and the ``grunt`` folder is updated to accomodate the new plugin structure

* adaptv4-cli
  * this is just a forked and published version of the issue/v4 branch of the usual adapt-cli repo (with a few minor updates), it lives [here](https://github.com/oliverfoster/adaptv4-cli)

### Outstanding
* core
  * ``adapt-contrib-dataLoader`` to allow for the loading of custom json files and to provide a pre-model stage
  * ``adapt-contrib-mpabc`` the menu, page, article, block, component and items abstraction, can be made of a data layer and a view layer
  * decide what to do with router, accessibility, etc
