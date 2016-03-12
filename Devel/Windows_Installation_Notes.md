There are several ways of having the CLAM framework and your apps working on windows.

Using prepackaged binaries
--------------------------

You can find packages in the [download page for Windows](http://clam-project.org/download-windows.html). Those include aplication installers for main CLAM applications: NetworkEditor, Chordata or Annotator. The next-next-next way.

For developers, there are some (experimental) precompiled packages containing either:

-   Just the 3rd party dependencies in order to develop CLAM in windows
-   3rd party dependencies and CLAM binaries, to develop your own project using CLAM.

Crosscompiling with Mingw from Linux
------------------------------------

[Devel/Windows\_MinGW\_cross\_compile](Devel/Windows_MinGW_cross_compile "wikilink")

This is the method we use to generate windows binaries and run the unattended tests. And the one more likely to work at a given moment.

Natively compiling with Mingw from Windows
------------------------------------------

[Devel/Windows MinGW build](Devel/Windows MinGW build "wikilink")

Those are user contributed methods to build it from Windows itself. Currently the document has many paths and has some dark zones. Feel free to enhance the document if you want to help the project.

In any case, using precompiled packages in the first section is recomended.

Natively compiling with MS Visual C (Obsolete)
----------------------------------------------

[Devel/Windows build](Devel/Windows build "wikilink")

MS-VisualC support was dropped from CLAM many years ago. The documentation is kept here just in case someone tries to bring it back, but it is all quite obsolete.
