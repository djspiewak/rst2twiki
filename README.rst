=========
rst2twiki
=========

Provides a very ugly Scala script which converts between DocUtils_ XML and `TWiki markup`_.
There is also a convenience wrapper for this script (``rst2twiki``)
which provides an interface very similar to the standard ``rst2...`` scripts
provided with the DocUtils library.

Please note that TWiki only implements a very small subset of DocUtils's markup
features.  Thus, the script is by necessity a bit of a hack.  It is of course
possible to simply use HTML to "fill in the gaps", but this would altogether
defeat the purpose of TWiki markup.  Instead, errors are given whenever an
unimplemented feature of `ReStructured Text`_ is used in the document.

.. _DocUtils: http://docutils.sourceforge.net/
.. _TWiki markup: http://zetkin.htu.tuwien.ac.at/cgi-bin/twiki/view/TWiki/TextFormattingRules
.. _ReStructured Text: http://docutils.sourceforge.net/rst.html


Usage
=====

Syntax::
    
    rst2twiki [input [output]]

If no ``input`` file is specified, the script will read from the standard input
stream and print to the standard out.  Likewise, if an ``input`` file is specified
without an ``output`` file, the ``input`` file will be read and the results
printed to the standard output stream.  This mirrors the syntax of the standard
``rst2...`` tools.


Special Features
================

A number of standard RST directives have been implemented, peering to their
corresponding TWiki features.  Most notably, these include:

* Image support (using the ``image::`` syntax)
* TWiki variable access

TWiki variables are used directly in markup with the ``%NAME%`` syntax.  However,
this is valid RST text and not special markup.  Thus, an alternative method for
accessing TWiki variables is required.  Fortunately, RST provides a convenient
syntax for just such directives.  The following sample shows the use of the
standard TWiki ``TOC`` variable::
    
    Sample 1
    ========
    
    `TOC`
    
    Lorem ipsum dolor sit amet...
    
This will be translated into the following TWiki markup (assuming no document
context)::
    
    ---+ Sample 1
    
    %TOC%
    
    Lorem ipsum dolor sit amet...


Performance
===========

Bad.  I didn't bother pre-compiling the Scala script, so you incur not only the
startup time of the JVM but also the entire compilation cycle each time you
invoke the script.  Fortunately, invocation doesn't need to be *that* speedy.
