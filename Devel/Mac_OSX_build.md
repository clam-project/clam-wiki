Install dependencies required by CLAM
-------------------------------------

### XCode Tools

-   Download and install XCode Tools following these instructions at macports web: <http://trac.macosforge.org/projects/macports>

  
The instructions include setting up the environment vars.

### Macports packages

-   First, download and install macports binaries that allow search and install other software (similar to apt-get)

` `[`http://svn.macports.org/repository/macports/downloads/`](http://svn.macports.org/repository/macports/downloads/)

-   Check that \~/.profile has been automatically modified with PATH pointing at /opt/local/bin

-   You maybe will need to start a new terminal to refresh the PATH value.

-   If you don't have wget installed it, consider doing it now, it always comes handy:

` $ sudo port install wget`

-   Install CLAM dependencies available in darwinports. This command will compile each packages from upstream (the original project site) and will automatically pull its dependencies:

`  $ sudo port install xercesc pkgconfig libsndfile libogg libmad libvorbis fftw-3 fftw-3-single cppunit id3lib portaudio`

-   python and scons are also in macports. They are needed as build tools. CLAM builds both with scons/python in darwinports and with the native packages.

` $ sudo port install scons`

### Packages not available in darwinports

**UPDATE: these packages are now available: portaudio, qt4-mac qt3-mac**

`  $ sudo port install portaudio qt4-mac`

#### Deprecated:

-   portaudio

`  $ wget `[`http://www.portaudio.com/archives/pa_snapshot_v19.tar.gz`](http://www.portaudio.com/archives/pa_snapshot_v19.tar.gz)

  
untar the package,

`  $ ./configure --disable-portaudio`
`  $ make `

  
and finally

`  $ sudo make install`

-   qt4-3.1 precompiled package - recommended

`  $ wget `[`http://ftp.iasi.roedu.net/mirrors/ftp.trolltech.com/qt/source/qt-mac-opensource-4.3.1.dmg`](http://ftp.iasi.roedu.net/mirrors/ftp.trolltech.com/qt/source/qt-mac-opensource-4.3.1.dmg)

  
Qt is installed in /usr/local/Trolltech//Qt-4.3.1 so you should define QTDIR accordantly

`  $ export QTDIR=/usr/local/Trolltech//Qt-4.3.1`

-   qt4.3.1 mac-free (not x11)

`  $ wget `[`http://ftp.iasi.roedu.net/mirrors/ftp.trolltech.com/qt/source/qt-mac-opensource-src-4.3.1.tar.gz`](http://ftp.iasi.roedu.net/mirrors/ftp.trolltech.com/qt/source/qt-mac-opensource-src-4.3.1.tar.gz)

  
and do the typical dance (macport devs devels informed us that a macport for qt4 is on the way). Note: qt is needed in order to build the applications, but you might not need it if all you want to do is build the lib. Since qt takes a long time to build and install it's worth skipping if you don't need it ;)

-   JackOSX and QJackCtrl

  
download JackOSX

[`http://jackosx.com/download.html`](http://jackosx.com/download.html)

  
mount it and follow instructions.

Then download QJackCtl

[`http://ardour.org/files/releases/QJackCtl-0.2.22.dmg`](http://ardour.org/files/releases/QJackCtl-0.2.22.dmg)

  
and read README for initial setup instructions.

CLAM sources
------------

You can choose either download a source tarball from the clam website:

  
<http://clam-project.org/download-source.html>

or get the latest version of subversion. In this case you'll also need to install subversion macport:

`  $ sudo port install subversion`
`  $ svn co `[`http://clam-project.org/clam/trunk`](http://clam-project.org/clam/trunk)` clam`

CLAM build
----------

Note that you need to disable LADSPA support when building for Mac. Doing this also means that the RunTimeFaustLibraryLoader cannot be built, but the SConscript in CLAM/scons/libs/core may not recognize this. You may want to apply the following fix:

`--- scons/libs/core/SConscript (revision 14373)`
`+++ scons/libs/core/SConscript (arbetskopia)`
`@@ -54,6 +54,7 @@`
` if not env.has_key('with_ladspa') or env['with_ladspa'] == False:`
`         blacklist.append( 'Ladspa.+' )`
`+        blacklist.append( 'RunTimeFaustLibraryLoader.+')`

Read the CLAM/INSTALL file if you're not familiar with the clam scons build sequence. Basically it's this:

` $ DEV_PFX=~/proj/clamdev # insert your chioice`
` $ mkdir -p $DEV_PFX`
` $ scons configure with_ladspa=no prefix=$DEV_PFX xmlbackend=none`
` $ scons`
` $ sudo scons install`

-   You may need to disable the XML backend as well and set the prefix somewhere else than /usr/local as indicated above

-   In case you are interested in building the Annotator app, you must build Annotator/vmqt first.

-   All applications should build and run. Inform clam-devel for any problem

-   applications packaging :
    -   just do 'scons bundle' and 'scons package'

-   Tip: use non system clam prefix (!= usr/local) for developing

Troubleshooting
---------------

-   If you encounter some errors while compiling the dependecies of CLAM with MacPorts, check that you have a default XCode installation plus the X11 SDK. Otherwise some missing SDKs can break compiliation of some packages.

-   I (Pau) got this weird error when executing clam-unrelated applications like gvim : "dyld: Symbol not found: \_\_cg\_jpeg\_resync\_to\_restart" it is a bug explained here: <http://bugs.wireshark.org/bugzilla/show_bug.cgi?id=1131> In my case I had to remove /opt/local/lib from DYLD\_LIBRARY\_PATH. You can also add export DYLD\_FALLBACK\_LIBRARY\_PATH=/opt/local/lib but it's not really necessary for CLAM

-   If you have problems with "scons configure" it might be that you are not using macports pkg-config. Check this

`  $ which pkg-config`
`  /opt/local/bin/pkg-config`
`  $ which uic`
`  /opt/local/bin/uic`
`  If you have non macport versions, change the PATH variable so that it starts with /opt/local/bin`

-   If some library fails to configure inform the clam-devel list but you can try with a subset. For example I first compiled clam with this clam.conf :

`prefix = '/Users/parumi/clamSandboxes/local'`
`release = 1`
`double = 0`
`sandbox = 1`
`checks = 1`
`release_asserts = 0`
`with_ladspa = 1`
`with_osc = 0`
`with_jack = 1`
`with_fftw = 1`
`with_nr_fft = 1`
`with_sndfile = 1`
`with_oggvorbis = 1`
`with_mad = 1`
`with_id3 = 1`
`with_portaudio = 1`
`with_portmidi = 0`
