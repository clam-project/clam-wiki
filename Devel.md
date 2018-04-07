This is the CLAM development wiki.

*Please, prepend 'Devel/' to any development pages you create here.*

-   [Development screenshots](Development screenshots) show last achievements on CLAM development branch.
-   [Testfarm web](http://clam-project.org/testfarm) shows the last commits and build and tests status in different platforms in real-time.
-   [GSoC 2007](GSoC 2007) [GSoC 2008](GSoC 2008) [GSoC 2009](GSoC 2009)
-   [Cia.vc, log of commits](http://cia.vc/stats/project/clam)
-   [Ohloh, project and developer stats](http://www.ohloh.net/projects/clam)

Subversion access
-----------------

Subversion tree contains CLAM libs and applications: NetworkEditor, Annotator, SMSTools and Voice2MIDI.

-   Open and read-only

`$ svn co `[`http://clam-project.org/clam/trunk`](http://clam-project.org/clam/trunk)` clam`

-   Active CLAM developers can also obtain commit access. See [rules for commit access](rules for commit access)

Getting involved
----------------

To get involved with CLAM development:

-   Subscribe to the [clam development mailing list](http://lists.clam-project.org/listinfo.cgi/clam-devel-clam-project.org).
-   Contact other CLAM developers at the \#clam channel of the FreeNode IRC network.
-   Blog (occasionally) about CLAM and syndicate your feed in the [clam planet](http://clam-project.org/planet).
-   Fill bugs in our [bug tracking tool](http://clam-project.org/bugs).
-   Fix bugs and send a [patch](#Prepare_and_apply_a_patch) ;-)
-   Send some good patches and get [svn commit access](Rules_for_commit access)

HOWTO's
-------

-   [Devel/Changelog release policy](Devel/Changelog release policy)
-   [Devel/Compile and install troubleshooting](Devel/Compile and install troubleshooting)
-   [Devel/Mac OSX build](Devel/Mac OSX build)
-   [Devel/Linux Gentoo build](Devel/Linux Gentoo build)
-   [Devel/Windows build](Devel/Windows build) with MS Visual Studio 2005 (obsolete)
-   [Devel/Windows MinGW build](Devel/Windows MinGW build) (under development)
-   [Devel/Windows MinGW cross compile](Devel/Windows MinGW cross compile) (the one working now for windows)
-   [Devel/Eclipse](Devel/Eclipse)
-   [Devel/Testing processing algorithms](Devel/Testing processing algorithms)
-   [Devel/CLAM idioms](Devel/CLAM idioms) C++ techniques we use over and over.
-   [Devel/Programming with Qt](Devel/Programming with Qt) tips and conventions for the CLAM code that uses the Qt framework.
-   [Devel/Faust support](Devel/Faust support)
-   [Devel/Blender support](Devel/Blender support)

Tasks
-----

-   [Devel/Release task list](Devel/Release task list): Every release check list.
-   [Devel/Current release TODOs](Devel/Current release TODOs) tasks due for the next release.

-   [Devel/Plugins TODOs](Devel/Plugins TODOs) short and mid-term tasks.
-   [Devel/Ladspa Plugins TODOs](Devel/Ladspa Plugins TODOs) short-term tasks. Mostly taken by Andreas Calvo.
-   [Devel/Faust integration TODOs](Devel/Faust integration TODOs) short-term tasks. Mostly taken by Andreas Calvo.
-   [Devel/Web TODOs](Devel/Web TODOs).
-   [SoC ideas](SoC ideas)
-   [Devel/Annotator TODO's](Devel/Annotator TODO's) includes GSoC project tasks taken by Wang Jun
-   [Devel/NetworkEditor TODO's](Devel/NetworkEditor TODO's)
-   [Devel/Infrastructure Refactoring TODO's](Devel/Infrastructure Refactoring TODO's)
-   [Devel/Spectral Transformations TODOs](Devel/Spectral Transformations TODOs) GSoC project tasks. Mostly taken by Hernán Ordiales
-   [Devel/Real-Time Spectral Model Synthesizer](Devel/Real-Time Spectral Model Synthesizer) GSoC project tasks. Mostly taken by Greg Kellum
-   [Devel/Chord Extraction TODO's](Devel/Chord Extraction TODO's) includes GSoC project tasks taken by Pawel Bartkiewicz
-   [Migration to clam-project.org](Migration to clam-project.org) tasks to migrate from iua.upf.edu hosting to community managed clam-project.org (hosted in Dreamhost)
-   [Buffer Ports](Buffer Ports) tasks to migrate proccesings from stream port to buffer port

### Hot Spots

Things that core developers are eager to address but still not being refined in small tasks. Of course they might need further discussion.

-   Split the processing library and add some plugins as libraries
-   Recursive networks
-   Run-time error handling and NetworkPlayer status feedback
-   Static network scheduling (Based on Pau's Thesis)
-   Hot wiring (allowing changing the network without stoping it)
-   Parametrized networks (Network configuration, propagated to processings' config)
-   Parameter propagation through the data-flow
-   Offline networks (visual prototyping on storing and retrieving data from Pools)
-   Extend the usage of new single representation spectrums to Spectral and SMS processings.
-   Ease widget plugin creation (binders for plots, type managers for configurations...)
-   Unify Do() return value semantics. One solution is do it void Do()
-   Implicit Consume and Produce when using ports (flow control does it).
-   Properly manage sampling rate
-   More plugins: VST, VSTi, LV2...
-   Better qt designer integration

### Hot Ideas

-   Network based music tracker (Controls being sequenced in time in a order list plus pattern way)

Prepare and apply a patch
-------------------------

We only give write access to the repository to developers with experience with clam development. But this not a problem since you can always send a patch to the devel-list, and clam-devs will take care of it (and we use to be quick).

We like receiving patches very much. Please do it even for very small things like improving the INSTALL file, or grammar corrections in the code comments. (So imagine how we like receiving bug-fixes!). Luckily, working with patches in subversion is very easy:

-   How to create a patch with your local modifications?

  
Change dir to the **root of your svn tree** (the directory containing CLAM/ and NetworkEditor/, etc.) and do

    $ svn diff > good-description.patch

You can also add new files in a patch by doing "svn add <newfile>". This adds the file locally -- doesn't need commit access.

However, if you do so, when the patch is commited by somebody else, svn up **will generate a confict** with the local added file. Therefore do "svn rm <newfile>" **before** updating for the applied patch.

If this fails, apply brute force: move the directory containing the new file elsewhere (out of the svn tree). And do restore it with "svn up".

-   How to apply a patch (that maybe somebody posted to the list)?

  
Change dir to the root of your svn tree and do

:

    $ patch -p0 < the.patch

We prefer the patches being sent to the **clam-devel mailing list**. The issue tracker will be used when patches can not be applied (or discarded) immediately and need to remain on hold for some time.

Pass the automatic tests
------------------------

Checkout the clam test-data repository:

`$ svn co `[`http://clam-project.org/clam_data/trunk`](http://clam-project.org/clam_data/trunk)` clam_data `

Change dir to CLAM/test and compile with **scons** providing the following options (no installation step is needed):

-   clam\_prefix --which is the prefix where you installed clam. /usr/local by default
-   test\_data\_path --the path to the clam\_data sandbox (local copy of the repository).

For example:

`$ scons clam_prefix=~/local test_data_path=~/clam_data`

Then execute unit and functional tests:

`$ ./UnitTests/UnitTests`
`$ ./FunctionalTests/FunctionalTests`
