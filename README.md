# adapt-v4-prototype
Readme for testing prototype.
Please post issues to [here](https://github.com/oliverfoster/adapt-v4-prototype/issues)


## Simple installation instructions
0. Use node 6+ (4 wont work)  
1. Open your terminal of choice and run:
```bash
npm install -g adaptv4-cli
```
2. Find a place to create a course and run:
```bash
adaptv4 create course my-adapt-course
```
3. Build it:
```bash
cd my-adapt-course
grunt dev
```

## Details

### Not working
* grunt
  * ``theme`` and ``menu`` parameters 
  * dependency inclusion for ``includes`` and ``excludes``
  * missing dependency warnings

### New bits
* core
  * is now 6 plugins [``adapt-contrib-boot``](https://github.com/oliverfoster/adapt-contrib-boot), [``adapt-contrib-core``](https://github.com/oliverfoster/adapt-contrib-core), [``adapt-contrib-data``](https://github.com/oliverfoster/adapt-contrib-data),[``adapt-contrib-drawer``](https://github.com/oliverfoster/adapt-contrib-drawer), [``adapt-contrib-navigation``](https://github.com/oliverfoster/adapt-contrib-navigation) and [``adapt-contrib-notify``](https://github.com/oliverfoster/adapt-contrib-notify). these 6 plugins are currently just an example of how the core can/should be sub-divided. i picked the easy bits.
  * the [``adapt-contrib-data``](https://github.com/oliverfoster/adapt-contrib-data) plugin loads the ``adapt/data/manifest.js``, then susequently all of the json files listed in the manifest. it provides an open data loader layer for Adapt to enable it to load custom json files (in addition to the standard ``config.json``, ``course.json``, ``contentObjects.json``, ``articles.json``, ``blocks.json`` and ``components.json``)
  * the adapt_framework branch for this prototype is [prototype4](https://github.com/adaptlearning/adapt_framework/tree/prototype4), the ``src/core`` folder is removed as is the ``src/index.html`` and the ``grunt`` folder is updated to accomodate the new plugin structure
  
* plugins (general)
  * are now flat, no more ``components/``, ``extensions/``, ``menu/`` or ``theme/`` folders
  * [can have specified dependencies ](https://github.com/oliverfoster/adapt-contrib-navigation/blob/master/bower.json#L12) which install automatically
  * can have a [``config.js``](https://github.com/oliverfoster/adapt-contrib-boot/blob/master/config.js) for each plugin rather than a global one for adapt_framework. this is hopefully only a temporary measure to ensure all of the plugins are able to configure the compilation process appropriately

* grunt
  * [``grunt-file-order``](https://github.com/cgkineo/grunt-file-order) was created to sort files in the build process
  * supports an extra plugin type of ``adapt-core`` to provide the first layer of the build process before ``adapt-component``, ``adapt-extension``, ``adapt-menu`` and ``adapt-theme``
  * grunt builds the flattened plugins in plugin-type-order (all ``adapt-core`` first, all ``adapt-theme`` last) and then in dependency-order (roots first, most dependant last)
  * the grunt config is simplified as all of the plugins are treated equally and there are no secondary rules for the deleted ``src/core/`` folder
  * grunt now rewrites the runtime requirejs dependency strings to accomodate the new layout. ``coreJS/`` will be rewritten into `adapt-contrib-core/js/`, etc. and warnings will be shown in the console for renamed namespaces
  * has some extra files to manage the plugins, to correct namespaces and to load requirejs configurations
  * has an extra task to build the ``adapt/data/manifest.json`` file, which contains a list of all the json files in the course folder, this ties into the [``adapt-contrib-data``](https://github.com/oliverfoster/adapt-contrib-data) plugin

* bower
  * the [``.bowerrc``](https://github.com/adaptlearning/adapt_framework/blob/prototype4/.bowerrc) file allows installation of plugins from multiple, layered bower repositories
  * there is a test 'version 4' bower repository at [http://adapt-bower-repository-v4.herokuapp.com/](http://adapt-bower-repository-v4.herokuapp.com/packages/) with the new version 4 plugins in it, its source code is [here](https://github.com/oliverfoster/node-bower-server) and it's running from heroku.
  * in this version of Adapt Framework, the ``adapt-bower-repository-v4`` plugins take prescendence over the ``adapt-bower-repository`` ones, but all of the core components should work seamlessly with this prototype

* adaptv4-cli
  * this is just a forked and published version of the issue/v4 branch of the usual adapt-cli repo (with a few minor updates), it lives [here](https://github.com/oliverfoster/adaptv4-cli/tree/issue/v4)

### Outstanding
* core  
  * remove the menu, page, article, block, component and items abstraction, can be made of a data layer and a view layer
  * decide what to do with router, accessibility, etc
