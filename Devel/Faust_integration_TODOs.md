milestones
----------

### milestone 1 "loading a hardcoded faust .dsp"

-   user edits faust.dsp with external editor ![](Done.png "fig:Done.png")
-   network editor provides a compile-and-load button

  
(for now it compiles an entire directory, as milestone 2)

-   errors goes to cerr ![](Done.png "fig:Done.png") (used a task runner widget instead)
-   on successful compilation:
    -   **refresh ladspa factory** ![](Done.png "fig:Done.png")
    -   refresh QTree ![](Done.png "fig:Done.png")
-   svg diagram window in NetworkEditor that reads the current svg file. ![](Done.png "fig:Done.png") (embedded on widget)

### milestone 2 "adding all .dsps in a dir and embedded svg"

-   scons that compiles all .dsp in a dir. ![](Done.png "fig:Done.png") (using 'make' with the original faust makefiles)

### milestone 3 "make the diagrams navigable and improve the compilation of faust modules"

-   make the svg diagrams navigable
-   improve the compilation using a standard makefile for faust on new .dsp source files

Related articles
----------------

-   [Devel/Faust support](Devel/Faust support "wikilink")

