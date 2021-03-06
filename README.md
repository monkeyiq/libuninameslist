libuninameslist – A Library of Unicode annotation data
======================================================

-   [Description](#description)
-   [Installation and Build instructions](#installation-and-build-instructions)
-   [Changelog](https://raw.github.com/fontforge/libuninameslist/master/ChangeLog)
-   [License](https://raw.github.com/fontforge/libuninameslist/master/LICENSE)
-   [See Also](#see-also)

Description
-----------

This program is updated for Nameslist.txt ver6.2 and ListeDesNoms.txt ver 5.0.
http://sourceforge.net/projects/libuninameslist/files/ is not kept up to date.

Nameslist.txt
The Unicode consortium provides [a file containing annotations on many unicode
characters.](http://www.unicode.org/Public/UNIDATA/NamesList.html) This library
contains a compiled version of this file so that programs can access these data
easily.

ListeDesNoms.txt
Is a seperate file which was translated from Nameslist.txt and currently outdated,
but contained here for backwards compatibility with older programs that may use it.

These libraries contain very large (sparse) arrays with one entry for each
unicode code point (U+0000–U+10FFFF). Each entry contains two strings, a name
and an annotation. Either or both may be NULL. Both libraries also contain a
(much smaller) list of all the Unicode blocks.

>     struct unicode_block {
>         int start, end;
>         const char *name;
>     };
>  
>     struct unicode_nameannot {
>         const char *name, *annot;
>     };
>   
>     extern const struct unicode_block UnicodeBlock[???];
>   
>     #define UNICODE_NAME_MAX    ???
>     #define UNICODE_ANNOT_MAX   ???
>     extern const struct unicode_nameannot * const *const UnicodeNameAnnot[];
>   
>     /* Index by: UnicodeNameAnnot[(uni>>16)&0x1f][(uni>>8)&0xff][uni&0xff] */

To keep both libraries slightly smaller, the beginning of lines starting with
TAB can be expanded with UTF-8 character substitutions as defined below:

>     /* At the beginning of lines (after a tab) within the annotation string, a */
>     /*  * should be replaced by a bullet U+2022 */
>     /*  x should be replaced by a right arrow U+2192 */
>     /*  : should be replaced by an equivalent U+224D */
>     /*  # should be replaced by an approximate U+2245 */
>     /*  = should remain itself */

With the default configure option chosen, this package will install one library
file and one header file. The library file is 'libuninameslist', and the header
is `<uninameslist.h>`. You can access to these three functions:

>     const char *uniNamesList_name(unsigned long uni);
>     const char *uniNamesList_annot(unsigned long uni);
>     const char *uniNamesList_NamesListVersion(void);

and for backwards compatibility for older programs that still use it, there is:

>     UnicodeNameAnnot[(uni>>16)&0x1f][(uni>>8)&0xff][uni&0xff].name

while the annotation string is:

>     UnicodeNameAnnot[(uni>>16)&0x1f][(uni>>8)&0xff][uni&0xff].annot

The name string is in ASCII, while the annotation string is in UTF-8 and is
also intended to be modified slightly by the having any `*` characters which
immediately follow a tab at the start of a line converted to a bullet
character. Etc.

If you choose to install the second library as well, then you will need to
use ./configure --enable-oldfrenchlib
This library only uses the older method so that it does not need to be
updated any further (latest library version now at 0.3). The header file
for this library is `<uninameslist-fr.h>`

Installation and Build instructions
-----------------------------------

Download the source from git:

```bash
$ git clone https://github.com/fontforge/libuninameslist.git;
$ cd libuninameslist
$ autoreconf -i
$ automake --foreign
$ ./configure
$ make
$ sudo make install | su
# make install
```

If you need to also include libuninameslist-fr, you will want to use:
$ ./configure --enable-oldfrlib

See Also
--------

-   [LibUnicodeNames](https://bitbucket.org/sortsmill/libunicodenames) - rewrite of this library, with Python module
-   [FontForge](http://fontforge.org/) - font editor application that this library was made for
-   [UMap](http://umap.sf.net/) - Find unicode characters and copy them to the clipboard 
