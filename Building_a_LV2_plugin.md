[LV2](http://lv2plug.in/) is an standard for free software audio plugins, and the succesor of [LADSPA](http://ladspa.org). CLAM is able to generate LV2 plugins based on CLAM networks. Audio and float control streams are currently well supported and there is some primitive support for generation or/and binding of Qt UIs.

Because LV2 support is not mature yet, you should expect changes on the way of LV2 plugins are generated, but, also because it is quite cheap this should not be a big deal. In any case, until the procedure becomes stable the better and most updated documentation is at the example you can find [here](http://clam-project.org/clam/trunk/CLAM/examples/PluginExamples/Lv2PluginExample/) which wraps a couple of networks as plugins in a single lv2 bundle.

That example use the clam\_lv2\_generator program whose [man page](http://clam-project.org/clam/trunk/CLAM/scripts/clam_lv2_generator.1) also provides some insight.

LV2 ui support
--------------

UI support is quite experimental and adhoc. The goal is supporting three use cases:

1.  Automated Qt ui generation given the properties of the network
2.  Custom Qt ui binded to the network by using special properties like Prototyper does (indeed the goal is reusing ui's).
3.  Hand programmed Qt interface, using facilities to interact with the network.

We are currently working in automatic generation (case 1) but using facilities that will enable case 2.

Notice that at time of writting this Qt support in LV2 is quite limited but we found a nice way of wrapping Qt widgets into Gtk.
