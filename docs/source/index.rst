`pyexcel` - Let you focus on data, instead of file formats
================================================================================

:Author: C.W.
:Source code: http://github.com/pyexcel/pyexcel.git
:Issues: http://github.com/pyexcel/pyexcel/issues
:License: New BSD License
:Development: |release|
:Released: |version|
:Generated: |today|

Introduction
-------------

**pyexcel** provides **one** application programming interface to read,
manipulate and write data in various excel formats. This library makes
information processing involving excel files an enjoyable task. The data in
excel files can be turned into :ref:`array or dict<a-list-of-data-structures>`
with minimal code and vice versa. This library focuses on data
processing using excel files as storage media hence fonts, colors and charts
were not and will not be considered.

The idea originated from the common usability problem: when an excel file
driven web application is delivered for non-developer users (ie: team assistant,
human resource administrator etc). The fact is that not everyone knows (or cares)
about the differences between various excel formats: csv, xls, xlsx are all
the same to them. Instead of training those users about file formats, this
library helps web developers to handle most of the excel file
formats by providing a common programming interface. To add a specific excel
file format type to you application, all you need is to install an extra pyexcel
plugin. Hence no code changes to your application and no issues with excel file
formats any more. Looking at the community, this library and its associated ones
try to become a small and easy to install alternative to Pandas.


Support the project
================================================================================

If your company has embedded pyexcel and its components into a revenue generating
product, please support me on github, `patreon <https://www.patreon.com/bePatron?u=5537627>`_
or `bounty source <https://salt.bountysource.com/teams/chfw-pyexcel>`_ to maintain
the project and develop it further.

If you are an individual, you are welcome to support me too and for however long
you feel like. As my backer, you will receive
`early access to pyexcel related contents <https://www.patreon.com/pyexcel/posts>`_.

And your issues will get prioritized if you would like to become my patreon as `pyexcel pro user`.

With your financial support, I will be able to invest
a little bit more time in coding, documentation and writing interesting posts.


Installation
-------------


You can install pyexcel via pip:

.. code-block:: bash

    $ pip install pyexcel


or clone it and install it:

.. code-block:: bash

    $ git clone https://github.com/pyexcel/pyexcel.git
    $ cd pyexcel
    $ python setup.py install

Suppose you have the following data in a dictionary:

========= ====
Name      Age
========= ====
Adam      28
Beatrice  29
Ceri      30
Dean      26
========= ====

you can easily save it into an excel file using the following code:

.. code-block:: python

   >>> import pyexcel
   >>> # make sure you had pyexcel-xls installed
   >>> a_list_of_dictionaries = [
   ...     {
   ...         "Name": 'Adam',
   ...         "Age": 28
   ...     },
   ...     {
   ...         "Name": 'Beatrice',
   ...         "Age": 29
   ...     },
   ...     {
   ...         "Name": 'Ceri',
   ...         "Age": 30
   ...     },
   ...     {
   ...         "Name": 'Dean',
   ...         "Age": 26
   ...     }
   ... ]
   >>> pyexcel.save_as(records=a_list_of_dictionaries, dest_file_name="your_file.xls")

And here's how to obtain the records:

.. code-block:: python

   >>> import pyexcel as p
   >>> records = p.iget_records(file_name="your_file.xls")
   >>> for record in records:
   ...     print("%s is aged at %d" % (record['Name'], record['Age']))
   Adam is aged at 28
   Beatrice is aged at 29
   Ceri is aged at 30
   Dean is aged at 26
   >>> p.free_resources()


Custom data rendering:

.. code-block:: python

    >>> # pip install pyexcel-text==0.2.7.1
    >>> import pyexcel as p
    >>> ccs_insight2 = p.Sheet()
    >>> ccs_insight2.name = "Worldwide Mobile Phone Shipments (Billions), 2017-2021"
    >>> ccs_insight2.ndjson = """
    ... {"year": ["2017", "2018", "2019", "2020", "2021"]}
    ... {"smart phones": [1.53, 1.64, 1.74, 1.82, 1.90]}
    ... {"feature phones": [0.46, 0.38, 0.30, 0.23, 0.17]}
    ... """.strip()
    >>> ccs_insight2
    pyexcel sheet:
    +----------------+------+------+------+------+------+
    | year           | 2017 | 2018 | 2019 | 2020 | 2021 |
    +----------------+------+------+------+------+------+
    | smart phones   | 1.53 | 1.64 | 1.74 | 1.82 | 1.9  |
    +----------------+------+------+------+------+------+
    | feature phones | 0.46 | 0.38 | 0.3  | 0.23 | 0.17 |
    +----------------+------+------+------+------+------+


Advanced usage :fire:
----------------------

If you are dealing with big data, please consider these usages:

.. code-block:: python

   >>> def increase_everyones_age(generator):
   ...     for row in generator:
   ...         row['Age'] += 1
   ...         yield row
   >>> def duplicate_each_record(generator):
   ...     for row in generator:
   ...         yield row
   ...         yield row
   >>> records = p.iget_records(file_name="your_file.xls")
   >>> io=p.isave_as(records=duplicate_each_record(increase_everyones_age(records)),
   ...     dest_file_type='csv', dest_lineterminator='\n')
   >>> print(io.getvalue())
   Age,Name
   29,Adam
   29,Adam
   30,Beatrice
   30,Beatrice
   31,Ceri
   31,Ceri
   27,Dean
   27,Dean
   <BLANKLINE>


Two advantages of above method:

#. Add as many wrapping functions as you want.
#. Constant memory consumption


.. testcode::
   :hide:

   >>> import os
   >>> os.unlink("your_file.xls")



For individual excel file formats, please install them as you wish:

.. _file-format-list:
.. _a-map-of-plugins-and-file-formats:

.. table:: A list of file formats supported by external plugins

   ======================== ======================= =================
   Package name              Supported file formats  Dependencies
   ======================== ======================= =================
   `pyexcel-io`_            csv, csvz [#f1]_, tsv,
                            tsvz [#f2]_
   `pyexcel-xls`_           xls, xlsx(read only),   `xlrd`_,
                            xlsm(read only)         `xlwt`_
   `pyexcel-xlsx`_          xlsx                    `openpyxl`_
   `pyexcel-ods3`_          ods                     `pyexcel-ezodf`_,
                                                    lxml
   `pyexcel-ods`_           ods                     `odfpy`_
   ======================== ======================= =================

.. table:: Dedicated file reader and writers

   ======================== ======================= =================
   Package name              Supported file formats  Dependencies
   ======================== ======================= =================
   `pyexcel-xlsxw`_         xlsx(write only)        `XlsxWriter`_
   `pyexcel-libxlsxw`_      xlsx(write only)        `libxlsxwriter`_
   `pyexcel-xlsxr`_         xlsx(read only)         lxml
   `pyexcel-xlsbr`_         xlsb(read only)         pyxlsb
   `pyexcel-odsr`_          read only for ods, fods lxml
   `pyexcel-odsw`_          write only for ods      loxun
   `pyexcel-htmlr`_         html(read only)         lxml,html5lib
   `pyexcel-pdfr`_          pdf(read only)          camelot
   ======================== ======================= =================


Plugin shopping guide
------------------------

Since 2020, all pyexcel-io plugins have dropped the support for python version
lower than 3.6. If you want to use any python verions, please use pyexcel-io
and its plugins version lower than 0.6.0.


Except csv files, xls, xlsx and ods files are a zip of a folder containing a lot of
xml files

The dedicated readers for excel files can stream read


In order to manage the list of plugins installed, you need to use pip to add or remove
a plugin. When you use virtualenv, you can have different plugins per virtual
environment. In the situation where you have multiple plugins that does the same thing
in your environment, you need to tell pyexcel which plugin to use per function call.
For example, pyexcel-ods and pyexcel-odsr, and you want to get_array to use pyexcel-odsr.
You need to append get_array(..., library='pyexcel-odsr').



.. _pyexcel-io: https://github.com/pyexcel/pyexcel-io
.. _pyexcel-xls: https://github.com/pyexcel/pyexcel-xls
.. _pyexcel-xlsx: https://github.com/pyexcel/pyexcel-xlsx
.. _pyexcel-ods: https://github.com/pyexcel/pyexcel-ods
.. _pyexcel-ods3: https://github.com/pyexcel/pyexcel-ods3
.. _pyexcel-odsr: https://github.com/pyexcel/pyexcel-odsr
.. _pyexcel-odsw: https://github.com/pyexcel/pyexcel-odsw
.. _pyexcel-pdfr: https://github.com/pyexcel/pyexcel-pdfr

.. _pyexcel-xlsxw: https://github.com/pyexcel/pyexcel-xlsxw
.. _pyexcel-libxlsxw: https://github.com/pyexcel/pyexcel-libxlsxw
.. _pyexcel-xlsxr: https://github.com/pyexcel/pyexcel-xlsxr
.. _pyexcel-xlsbr: https://github.com/pyexcel/pyexcel-xlsbr
.. _pyexcel-htmlr: https://github.com/pyexcel/pyexcel-htmlr

.. _xlrd: https://github.com/python-excel/xlrd
.. _xlwt: https://github.com/python-excel/xlwt
.. _openpyxl: https://bitbucket.org/openpyxl/openpyxl
.. _XlsxWriter: https://github.com/jmcnamara/XlsxWriter
.. _pyexcel-ezodf: https://github.com/pyexcel/pyexcel-ezodf
.. _odfpy: https://github.com/eea/odfpy
.. _libxlsxwriter: http://libxlsxwriter.github.io/getting_started.html

.. table:: Other data renderers

   ======================== ======================= ================= ==================
   Package name              Supported file formats  Dependencies     Python versions
   ======================== ======================= ================= ==================
   `pyexcel-text`_          write only:rst,         `tabulate`_       2.6, 2.7, 3.3, 3.4
                            mediawiki, html,                          3.5, 3.6, pypy
                            latex, grid, pipe,
                            orgtbl, plain simple
                            read only: ndjson
                            r/w: json
   `pyexcel-handsontable`_  handsontable in html    `handsontable`_   same as above
   `pyexcel-pygal`_         svg chart               `pygal`_          2.7, 3.3, 3.4, 3.5
                                                                      3.6, pypy
   `pyexcel-sortable`_      sortable table in html  `csvtotable`_     same as above
   `pyexcel-gantt`_         gantt chart in html     `frappe-gantt`_   except pypy, same
                                                                      as above
   ======================== ======================= ================= ==================

.. _pyexcel-text: https://github.com/pyexcel/pyexcel-text
.. _tabulate: https://bitbucket.org/astanin/python-tabulate
.. _pyexcel-handsontable: https://github.com/pyexcel/pyexcel-handsontable
.. _handsontable: https://cdnjs.com/libraries/handsontable
.. _pyexcel-pygal: https://github.com/pyexcel/pyexcel-chart
.. _pygal: https://github.com/Kozea/pygal
.. _pyexcel-matplotlib: https://github.com/pyexcel/pyexcel-matplotlib
.. _matplotlib: https://matplotlib.org
.. _pyexcel-sortable: https://github.com/pyexcel/pyexcel-sortable
.. _csvtotable: https://github.com/vividvilla/csvtotable
.. _pyexcel-gantt: https://github.com/pyexcel/pyexcel-gantt
.. _frappe-gantt: https://github.com/frappe/gantt

.. rubric:: Footnotes

.. [#f1] zipped csv file
.. [#f2] zipped tsv file

For compatibility tables of pyexcel-io plugins, please click `here <http://pyexcel-io.readthedocs.io/en/latest/#id5>`_

.. table:: Plugin compatibility table

    ======== ========== ============= ==================== ============= =============
    pyexcel  pyexcel-io pyexcel-text  pyexcel-handsontable pyexcel-pygal pyexcel-gantt
    ======== ========== ============= ==================== ============= =============
    0.6.5+   0.6.2+     0.2.6+        0.0.1+               0.0.1         0.0.1
    0.5.15+  0.5.19+    0.2.6+        0.0.1+               0.0.1         0.0.1
    0.5.14   0.5.18     0.2.6+        0.0.1+               0.0.1         0.0.1
    0.5.10+  0.5.11+    0.2.6+        0.0.1+               0.0.1         0.0.1
    0.5.9.1+ 0.5.9.1+   0.2.6+        0.0.1                0.0.1         0.0.1
    0.5.4+   0.5.1+     0.2.6+        0.0.1                0.0.1         0.0.1
    0.5.0+   0.4.0+     0.2.6+        0.0.1                0.0.1         0.0.1
    0.4.0+   0.3.0+     0.2.5
    ======== ========== ============= ==================== ============= =============


.. table:: A list of supported file formats

    ============ =======================================================
    file format  definition
    ============ =======================================================
    csv          comma separated values
    tsv          tab separated values
    csvz         a zip file that contains one or many csv files
    tsvz         a zip file that contains one or many tsv files
    xls          a spreadsheet file format created by
                 MS-Excel 97-2003 
    xlsx         MS-Excel Extensions to the Office Open XML
                 SpreadsheetML File Format.
    xlsm         an MS-Excel Macro-Enabled Workbook file
    ods          open document spreadsheet
    fods         flat open document spreadsheet
    json         java script object notation
    html         html table of the data structure
    simple       simple presentation
    rst          rStructured Text presentation of the data
    mediawiki    media wiki table
    ============ =======================================================


Usage
------

.. testcode::
   :hide:

   >>> import pyexcel
   >>> # make sure you had pyexcel-xls pip-installed
   >>> a_list_of_dictionaries = [
   ...     {
   ...         "Name": 'Adam',
   ...         "Age": 28
   ...     },
   ...     {
   ...         "Name": 'Beatrice',
   ...         "Age": 29
   ...     },
   ...     {
   ...         "Name": 'Ceri',
   ...         "Age": 30
   ...     },
   ...     {
   ...         "Name": 'Dean',
   ...         "Age": 26
   ...     }
   ... ]
   >>> pyexcel.save_as(records=a_list_of_dictionaries, dest_file_name="your_file.xls")

Suppose you want to process the following excel data :

.. pyexcel-table::

   ---pyexcel:example table---
   Name,Age
   Adam,28
   Beatrice,29
   Ceri,30
   Dean,26


Here are the example usages:

.. code-block:: python

   >>> import pyexcel as pe
   >>> records = pe.iget_records(file_name="your_file.xls")
   >>> for record in records:
   ...     print("%s is aged at %d" % (record['Name'], record['Age']))
   Adam is aged at 28
   Beatrice is aged at 29
   Ceri is aged at 30
   Dean is aged at 26
   >>> pe.free_resources()

.. testcode::
   :hide:

   >>> import os
   >>> os.unlink("your_file.xls")

Design
--------------------

.. toctree::

   design
   capability
   architecture

New tutorial
--------------
.. toctree::

   quickstart
   two-liners
   iodrivers
   webdev
   renderers
   sheet
   book
   database

Old tutorial
--------------
.. toctree::

   tutorial_file
   tutorial06
   tutorial_data_conversion
   attributes.rst
   bigdata
   tutorial
   tutorial05
   tutorial02
   tutorial03
   tutorial04

Cook book
----------

.. toctree::

   cookbook
   sources

Real world cases
-------------------

.. toctree::

   answers
   showcases/db_injection

API documentation
------------------

.. toctree::
   :maxdepth: 2

   api
   iapi


Developer's guide
------------------

.. toctree::

   guide
   logging
   pyinstaller
   plugin_howto

Change log
-------------------

.. toctree::
   :maxdepth: 2

   migration_guide
   changelog


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
