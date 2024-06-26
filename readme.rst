Bugzilla Schema Doc Generation
==============================

0. Bitrot Warning
-----------------

This repo is now (2024-04-29) some years out of date.
`The Bugzilla team <https://github.com/bugzilla>`_ have
`forked it <https://github.com/bugzilla/bugzilla-schema>`_ and are
actively working on that fork; I recommend you follow it there.

1. Introduction
---------------

This repository contains the code and documents used to generate
the `online Bugzilla schema documentation <http://www.ravenbrook.com/tool/bugzilla-schema/>`_,
produced as a side-benefit
of the `Perforce Defect Tracking Integration <http://www.ravenbrook.com/project/p4dti/>`_ project.

Please note:

- this has not been substantially updated since 2009, and the last
  schema contained in the data is for Bugzilla 3.3.4.  We don't have
  an ongoing business case for maintaining it.

- it was written and has been maintained in a very ad-hoc fashion, and
  the job which it aims to achieve is intrinsically complex.  We are
  not proud of this code.

Please fork it and bring it up to date!

2. Index
--------

================== ====================================================================
File               Description
================== ====================================================================
pickle_schema.py   A Python module to interrogate MySQL to obtain a live database
                   schema, and to write a "pickled" version of that schema into a file
                   in the "pickles" directory.
pickles            A directory containing pickled versions of every Bugzilla
                   database schema, generated by pickle_schema.py.  Each pickle is
                   named after the first version of Bugzilla which had that
                   schema.
get_schema.py      A Python module to read a pickled schema from the "pickles"
                   directory, annotate it with data from schema_remarks.py, and convert
                   it to a canonical Python dictionary form.
schema_remarks.py  A Python module defining all the comments and running text which
                   are ever used in the generated documentation (excluding
                   automatically-generated text such as field names, types, attributes,
                   and notes on schema changes).  Also lists the schemas available in
                   "pickles", and provides the mapping from Bugzilla version to schema
                   version name.
make_schema_doc.py The main Python documentation generation module.  Uses the
                   unpickled schemas fetched by get_schema.py, compares them to
                   identify schema changes and colour the resulting charts, processes
                   the text to include comments appropriate to the range of schemas
                   requested, and automated comments reflecting the schema version
                   ranges specific to particular pieces of commentary, and produces the
                   resulting HTML document.
index.py           The front-end CGI script which presents a form, validates input
                   through the form, and drives make_schema_doc to produce the schema
                   documentation.
index.cgi          A tiny Python script which uses index.py to do all of the CGI
                   work.  The two files are separated so that the source of index.py
                   can be published directly through the same web interface as the
                   generated schemas.
================== ====================================================================

3. Requirements
---------------
For hosting:

- Python
- a web server that can run Python CGI.

For updating:

- The above, and also:
- The ability to create Bugzilla installations for different versions of Bugzilla,
  using MySQL.
- MySQLdb.
- Understanding of Perl, and of Bugzilla internals.
- Patience.

For guidance on updating the tool to handle newer Bugzilla releases, see updating.rst.
