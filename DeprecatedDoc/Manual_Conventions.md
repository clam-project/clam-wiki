[Back to Index](DeprecatedDoc/CLAMUserManual "wikilink")

Indenting code
==============

These guidelines aims that the code will be always well indented with the indentation size that each programmer prefers and keeps some parts of the code vertically aligned.

-   Use only tabs to indent the beginning of each line.
-   Use only spaces to align vertically (For example when writing variables declarations in two aligned columns or splitting a long line)
-   If the aligned word is the first one in the line, you must keep the same number of tabulators that has the line you want to align with.

On the following example [tab] represents tabulators and . represents spaces used for alignment.

`[tab] [tab] double.a;`
`[tab] [tab] int....b;`
`[tab] [tab] if (a`<b && b<c) {
 [tab] [tab] [tab] std::cout << "a is less than c";
 [tab] [tab] [tab] ..........<< std::endl;

Ensure that your editor does not change tabulator by spaces or spaces by tabulators.
Some hints about some used editors:

    * MS VisualC++ : Tools->`Options->Tabs-> FileType:C/C++, Keep Tabs, AutoIndent: Smart`
`   * Emacs (permanent configuration): Add something like this to your .emacs file: (setq c-default-style '((other . "bsd"))) (custom-set-variables '(tab-width 4))`
`   * Emacs (per file/region):  M-x c-set-style [ret] bsd (to convert a region, do C-space at the begining, move the cursor, then do M-x indent-region`

Naming conventions
==================

`   * When an identifier is composed of several words, they must be appended together (without "_') and distinguishing each word with the initial in upper-case.`
`   * Functions and methods starts with upper-case.`
`   * Variables and members start with lower-case.`
`   * Normal members should start with a lower-case 'm' like in mMemberName`
`   * Static members should start with a lower-case 's' like in sMemberName`
`   * Identifiers inside an enum declaration must start with a lowercase 'e' like in enum MusicalStyles { eRock, ePop, eClassic };`
`   * Dynamic Types members: Mustn't start with 'm' or 's' and in upper-case. Dynamic types are implemented using macros that will create code for the member's accessors. So, when registering a dynamic type member like DYN_ATTRIBUTE(0,public, ComplexArray, Array`<Complex< TData>` >) keep in mind that the identifier (3rd parameter) will be used for creating the methods: GetComplexArray() and so on.`

§ It's possible that in the future we'll change to a template-based implementation, so that it will turn to a normal member declaration. Then the naming convention is likely to be like dComplexArray.

Programming style
=================

Use const whenever is possible:

`   * Declare const a member function (method) when it doesn't modify the object`
`   * Declare const the parameters that are not going to be modified inside the function`
`   * Declare const the return if it is something of the object we don't want to be modified outside`
`   * Use const & parameter passing instead of by value, for efficiency`

In general is better to pass parameters by reference than by pointers, because we implicitly are not allowing delete the object.

Error Conditions
================

`   * See Error notification and managing for more details.`
`   * If an error condition is to be handled by the function caller throw an exception and publish it on the function prototype with the throw keyword.`
`   * If an error condition is not defined as interface use CLAM_ASSERT.`
`   * Every published exception must be caught. Not catching a published exception is a bug.`
`   * Catch the exceptions locally. Don't group several functions that may throw exceptions on the same try clause.`
`   * Catch the exceptions concretely. Catch for one concrete type of exception so you can recover for it.`
`   * When you can't handle the exception locally, translate the exception to the caller context and throw.`

Debugging aids
==============

There are some useful coding techniques for finding "well hidden" errors very quickly, that otherwise might escape tests but can appear at most undesirable circumstances: sometimes this is called defensive programming. Here we give some guidelines:

`   * Systematically check that the state is consistent with asserts. Use the macro CLAM_ASSERT(bool_expression, info message) for that. Its code will be removed when compiling with optimization options, see the Error notification and managing for more details.`
`   * The precondition and postcondition of a method are the restrictions --about the parameters and object state-- that a method must satisfy at the entry and return points respectively. Is very recommendable to use asserts for checking these conditions.`
`   * An invariant of an object is the set of restrictions that the state of the object will always satisfy during its life. Write a method: bool FulfillsInvariant(void) that checks the most relevant restrictions of the invariant and uses it like this : CLAM_ASSERT(FulfillsInvariant(), "anything..."). Moreover, is very useful that FulfillsInvariant() call the same method of its members if it exist.`
`   * Another possible signature for this method is : void Fulfillsinvariant(void) throw ErrAssertionFailed. In that case the invariant conditions will be checked using CLAM_ASSERT and no boolean return is necessary, this can be an advantage if we want each condition to have a different informative message.`
`   * Companion removable code of a CLAM_ASSERT must be enclosed by CLAM_BEGIN_CHECK and CLAM_END_CHECK macros that remove de code when some optimization options are enabled.`

`CLAM_BEGIN_CHECK`
`for (int i=0; i<N; i++) {`
`  CLAM_ASSERT(array[i].IsValid(),"Invalid Element                `
`                                  Found");`
`}`
`CLAM_END_CHECK   `

See examples of FulfillsInvariant() in the code of the library classes DynamicType whose file is placed in src/Base/ or any XMLAdapter class that can be found at src/Storage/XML.
