Who can get commit access?
--------------------------

Any active Clam developer can get a SVN account. Normally one is considered an 'active' developer after sending several patches to the mailing lists for review and getting them accepted. If you do so, usually core developers will ask you if you want commit access. That is a user and a password to commit into the svn.

If you get that point, remember to follow the following guidelines on your first commits.

When to directly commit?
------------------------

-   Small refactorings, typos, trivial fixes...

-   Bigger changes but in safer places: examples, apps or processings not used by other people at the moment.

For any commit check that:

-   It doesn't break [testfarm](http://clam.iua.upf.edu/testfarm/) (if it does fix it quickly or revert the commit). Of course, do not rely on testfarm to see if tests pass, do it locally before commiting.

-   You write a proper changelog (use itemized "\*" lines)

-   Different aspects goes into different commits.

-   Send a "last commits" notice to the mailing list explaining the last recent commits (sending changelog might be fine).

When not to directly commit?
----------------------------

-   When the change is larger and effect existing code (maybe client code not in SVN). In this case the patch \*must\* be send to clam-devel list for a previous review of a core developer. Check that tests pass and apps build before sending patches.

When a first patch has been approved the rules for follow-up patches in a discussed and agreed direction are slightly relaxed.
