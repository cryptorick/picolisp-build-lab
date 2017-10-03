# Picolisp Build Lab

This is a development area for me to test new scripts and things to
build Picolisp.

## `src/Makefile`

`src/Makefile` is the makefile for the 32-bit build.  It is written in
such as way that it is dependent on running with GNU make (`gmake`)
only.  Hence, people who don't run on GNU systems, have to install
`gmake`.  So, the motivating question then becomes: why not write
`src/Makefile` so that it can be run with the system `make`?  That's
what [this version of `src/Makefile`](src/Makefile) is about.

*Status*.  So far, this version works with GNU make and the BSD make
 on OpenBSD.  Still need to test this with the system `make` on the
 other BSDs and on other less-popular (but still well-used) systems
 (like AIX, HP-UX, etc.) which are supported by the good folks in the
 Picolisp community.

## TODO

Idk.

Maybe work on `src64/Makefile` next?

Anything else?
