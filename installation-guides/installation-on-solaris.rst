=======================
Installation On Solaris
=======================

If using a Solaris system, mod_wsgi can be compiled from source code by
following standard installation instructions.

For descriptions of general problems that may be encountered during
installation on Solaris, see the documentation:

* :doc:`../installation-issues/index`

The following Solaris specific issues can also arise.

Missing Shared Libraries
------------------------

By default, compilers on the Solaris platform will not build a dynamically
loadable object where libraries required are only available as a static
library. This can particularly be a problem if the version of Python being
used was not configured and installed so as to produce a shared library.

The error output from the compiler when such a problem arises will be::

    /home/shauer/sys/apache-2.2.6/build/libtool --silent --mode=link gcc -o \
     mod_wsgi.la  -rpath /home/shauer/sys/apache-2.2.6/modules -module \
     -avoid-version    mod_wsgi.lo -L/usr/local/lib/python2.5/config \
     -lpython2.5 -lresolv -lsocket -lnsl -lrt -ldl
    Text relocation remains                         referenced
       against symbol                  offset      in file
    <unknown>                           0x1030      /usr/local/lib/python2.5/config/libpython2.5.a(floatobject.o)
    <unknown>                           0x1034      /usr/local/lib/python2.5/config/libpython2.5.a(floatobject.o)
    <unknown>                           0x1038      /usr/local/lib/python2.5/config/libpython2.5.a(floatobject.o)
    <unknown>                           0x103c      /usr/local/lib/python2.5/config/libpython2.5.a(floatobject.o)
    <unknown>                           0x1040      /usr/local/lib/python2.5/config/libpython2.5.a(floatobject.o)
    <unknown>                           0x1044      /usr/local/lib/python2.5/config/libpython2.5.a(floatobject.o)

As explained in section 'Lack Of Python Shared Library' of
[InstallationIssues Installation Issues], the prefered remedy is to recompile
and reinstall Python so that it provides a shared library. If for some reason
this is not possible, then the 'Makefile' for mod_wsgi should be modified
after having run 'configure', with the 'LDFLAGS' variables being
ammended to pass the '-mimpure-text' option through to the linker in
addition to other options also being used::

    LDFLAGS = -mimpure-text ....

Do note that although this will work, it is not at all recommended. This is
because compilers on Solaris seem to generate much much larger object files
than other platforms. This means that the static Python library can be 7MB
or more in size. If this has to be linked statically into the mod_wsgi,
when loaded into memory it can result very large memory usage by the Apache
child processes even before any WSGI application is loaded. In other words,
rebuild your Python installation and use a shared library instead.

Precompiled Binaries
--------------------

Precompiled binary packages are in the process of being made available from:

  http://www.opencsw.org/

If not yet in the main package area, see the latest packages uploaded for
testing.

  http://mirror.opencsw.org/testing.html
