This article explains the Blender integration on CLAM. You could find the needed Blender python scripts [on the 'spacialization' plugin directory of the SVN](http://clam-project.org/clam/trunk/CLAM/plugins/spacialization/blender/).

The following scripts uses the blender objects included in the 'AudioSources' and 'AudioSinks' groups as general convention.

Defining the Blender scene
--------------------------

### Adapting a previous Blender scene

If you have an existing Blender scene, just rename the objects that you want to use, to have the "listener" and "source" substrings. Then import the script that you want to use from the following ones:

### Creating a new Blender scene

If you want to start a new scene, you could import the scene\_maker script and run it once. Or directly, call blender with -P from the "CLAM/plugins/spacialization/blender" path:

`blender -P scene_maker.py`

It would ask how many sources and sinks you want to generate:

![](Blender scene maker1.png "Blender scene maker1.png")

and generate UVspheres as generic models, included on the specific groups:

![](Blender scene maker2.png "Blender scene maker2.png")

![](Blender scene maker graphic.png "Blender scene maker graphic.png")

Sending positions through OSC
-----------------------------

The 'BlenderOSCSender.py' script allows to send OSC messages the sources and listeners positions/rotatios through OSC using '[OSC.py](http://clam-project.org/clam/trunk/CLAM/plugins/osc/oscpython/OSC.py) (OSC client module for python by Stefan Kersten, you have to put a copy on your home/src/liblo or have it on relative path ../../osc/oscpython). The script should be linked to any source or listener object to ObjectUpdate event, and to FrameChanged event from scene, to send a sync message every frame. The sending OSC path is '/[SpatDIF](http://www.jamoma.org/wiki/SpatDIFSpatialSoundDescriptionInterchangeFormat)/object/number/xyz/type', where 'object' is the type of audio object ('source' or 'listener'), number is the relative number within the correspondent type group, and 'type' is rotation (azimuth/elevation/roll angles) or location (x/y/z).The default used OSC port is 7000, but you can change that adding '\_pxxxx' at the end of the blender (sources/listeners) objects, where xxxx is the number of the wanted port.

### Configuring the sender

You could import the BlenderOSCSender.py in your list of Text blender buffers and assign on the desired event (FrameChanged on a scene, ObjectUpdate on a listener or a source):

![](Blender link OSCSender on framechanged event.png "fig:Blender link OSCSender on framechanged event.png") ![](Blender link OSCSender on ObjectUpdate event.png "fig:Blender link OSCSender on ObjectUpdate event.png")

When you generate the Blender scene using the scene\_maker script, the OSCSender will be automatically added to the propers events.

### Exporting the scene to CLAM network receivers

First at all, you will need to compile and install the OSC CLAM plugin, as MultiLibloSources are used to receive the OSC messages. To generate a CLAM network OSC monitor for that scene, you could call the 'network\_scene\_exporter.py' script (which is automatically called by the scene\_maker, and applied to the ObjectUpdate object and FrameChanged scene events). It will ask for a network filename:

![](Blender scene maker filedialog network.png "Blender scene maker filedialog network.png")

And you will obtain a Network like this:

![](Clam osc receivers for blender scene.png "Clam osc receivers for blender scene.png")

To make it work, just run your Blender animation (alt-a), and the monitors would be updated to the objects positions, and if is called by a FrameChanged event the CLAM Network would be also show the actual frame number.

Exporting a choreographic sequencer file
----------------------------------------

The blender choreographic sequencer file exporter is included on the 'choreofile\_exporter.py' script, and generates the choreographic sequencer file for one listener and one source selected objects, and a sequencer monitor network (using network\_scene\_exporter).

First you will need to select **just one** listener **and one** source object (select with right click, add selection with shift-right click), and run the script ("alt-p" on the Text Editor using choreofile\_exporter).

The script will ask for a sequencer file name:

![](Blender exporting choreoSequencer file.png "Blender exporting choreoSequencer file.png")

A Network with the same base name of the choreographic file will be created, with a choreo sequencer processing configured to read the created sequencer file at the proper controls per second (using blender animation frames per second):

![](Blender generated network ChoreoSequencer.png "Blender generated network ChoreoSequencer.png")

![](ChoreoSequencerConfig BlenderScene.png "ChoreoSequencerConfig BlenderScene.png")

A dummy animation
-----------------

If you know something of Blender, maybe will found the previous scripts usefuls as templates for new Blender-CLAM integrated scenes. But if you don't know how to make animations in Blender, you still could prove this scripts just calling the demo\_scene\_maker script from your CLAM blender scripts path:

`blender -P demo_scene_maker.py`

That would be generate the same scene and network than scene\_maker, but then will add a circular trajectories on sources. So, just play the animation with "alt-a", and test the networks! ![](BlenderDemoScene.png "fig:BlenderDemoScene.png")
