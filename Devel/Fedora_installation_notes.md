The INSTALL files provide most of the information on dependencies. This page points out just some things not present in such a file.

An FC6 rpm for portaudio 19 is needed in order to activate it. Anyone?

-   Qt3, QTDIR=/usr/lib/qt-3.3
-   Qt4, QTDIR=/usr
-   Qt4, QTDIR=/usr/lib/qt4 (Fedora core \< 6)

Under Fedora 12, the installer was trying to use old qt binaries. You need to set: QTDIR=/usr/lib/qt4
