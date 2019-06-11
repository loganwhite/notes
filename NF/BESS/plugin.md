## Remove plugins
Buiding the plugins will add the plugin folder into the building configuration file, and next time even if you don't want to add build the plugin and delete the plugin folder, the building system will not automatically clean the configuration. We can use the following command to rebuild the bess
```
# ./build.py --reset-plugins
```
