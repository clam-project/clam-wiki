Just taking a clam-devel mail as a start. It should evolve into a kind of FAQ.

-   double check that you don't have a clam installation in /usr. *ls /usr/lib/libclam\** should give no file.

-   you said you where compiling the "examples". you mean CLAM/examples, right? (or apps like NetworkEditor?) check that prefixs in both CLAM/examples/options.cache and CLAM/scons/libs/clam.conf matches

-   you had different clam installs. this should not be a problem but just in case clean up the install (if not already done). i'm not sure if scon -c works properly so i do it by hand. and then use svn version. (assuming prefix==/usr/local)

`rm /usr/local/lib/libclam*`
`rm /usr/local/lib/pkgconfig/clam_*.pc`
`rm -rf /usr/local/include/CLAM`

-   touching a .cxx does **not** force recompilation! scons is too smart: it does not use timestamps but the content of the source file to decide when it has changed. removing compilation files under CLAM/scons/libs/\* doesn't help either on forcing reinstall. the only think you can do is clean your installed files.

-   more radical things you can do: *scons -c* to clean intermediate compilation files or remove your source tree and do a clean checkout.

-   If compilation gives you errors while running 'moc', about a missing interface, probably you have the QTDIR missconfigure. Assure you have it defined to point a directory where moc can find things such include/QtCore, plugins/designer, mkspecs, bin/moc... Chech the compilation notes for your platform.

