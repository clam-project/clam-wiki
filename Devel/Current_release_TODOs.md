1.4.0
-----

### Debian packaging

-   ![](Done.png "fig:Done.png") How to install the examples. -\> Uncompressed
-   ![](Done.png "fig:Done.png") neutral (or debian) distribution in changelog
-   ![](Done.png "fig:Done.png") Make packages non-native
-   ![](Done.png "fig:Done.png") Fill separate ITP bugs in debian for the NetworkEditor
-   ![](Done.png "fig:Done.png") Fill separate ITP bugs in debian for the Chordata
-   ![](Done.png "fig:Done.png") Fill separate ITP bugs in debian for the clam-plugins
-   ![](Done.png "fig:Done.png") Find the GPLv2 only and MIT license files.
    -   ![](Done.png "fig:Done.png") Acknoledge GPLv2 code comming from zynaddsubfx in plugins/GuitarEffects
    -   ![](Done.png "fig:Done.png") Acknoledge MIT like code comming from RTMIDI/Audio
-   ![](Done.png "fig:Done.png") Remove debian/ folders from upstream tarballs
-   ![](Done.png "fig:Done.png") What to do with VCS fields? -\> point to debian folders
-   ![](Done.png "fig:Done.png") Man pages for the extra binaries
-   ![](Done.png "fig:Done.png") Signing the DSC files
-   Getting an sponsor for key packages (clam, plugins, chordata and networkeditor)
-   Annotator: Packaging python scripts
-   Annotator: Adding missing manpages
-   SMSTools: Adding missing manpages

### Other

-   Incorporate resample plugin into testfarm and debian
-   Fix the zero sample on Resample
-   Build windows packages for Voice2MIDI
-   How-to: minimal processing with audio ports
-   How-to: processing composite
-   Clean old neteditor tests?
-   Make Assert handler thread safe. Store message in network.
-   Windows path for plugins
-   Look for plugins at clam install path
-   testfarm
    -   Unify testfarm client scripts
    -   nightly execution: doxygen, release, double, snapshot up

1.3.0
-----

-   ![](done.png "fig:done.png") Add GuitarEffects plugin into linux testfarm

1.2.0
-----

-   ![](Done.png "fig:Done.png") Package the plugins
-   ![](Done.png "fig:Done.png") Add a README for each plugin
-   ![](Done.png "fig:Done.png") Migration guide: FactoryRegistrator instead of Factory::Registrator
-   ![](Done.png "fig:Done.png") **Update How-to new processing**
-   ![](Done.png "fig:Done.png") Testfarm: Add windows mingw client
-   ![](Done.png "fig:Done.png") Testfarm: Include plugin compilation

1.1
---

-   New InControlInt\_\_XXX prototyper bind name
-   InControl\_\_XXX maps the InControl bounds
-   Add InControl bounds to every processing used in prototyped examples
-   Update examples (.ui files)
-   NetworkEditor faust\_dir option
-   MSVC NetworkEditor installer (thanks Pol Pros)

0.99 (aka 1.0 preview)
----------------------

-   **hi:** windows do not compile. PA errors 'paWASAPI' and 'paAudioScienceHPI' undeclared
-   check debian/ubuntu packages
-   check FC packages
-   check mac osx dmgs.
-   check windows installers

