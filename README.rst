Creative Commons i18n Tools
===========================

:Author: Nathan R. Yergler <nathan@creativecommons.org>
:Organization: `Creative Commons <https://creativecommons.org/>`_
:Copyright:
   2007, Nathan R. Yergler, Creative Commons;
   licensed to the public under the `Expat/MIT license
   <http://opensource.org/licenses/mit-license.php>`_.


Package Contents
================

This package provides the localization dataset for the Creative Commons
website, along with a set of tools to manage translations. For historical
reasons the CC site templates use abstract string identifiers (ie, "deed.by")
instead of the customary English text. Many tools are available for gettext
catalogs which assume that the English text is the identifier; the tools
included convert between the different formats.

In addition to the tools, the dataset is maintained in two different formats.
The ``po`` directory contains the gettext .po files in the traditional format,
with English text as the string identifiers. The ``i18n`` directory contains
the gettext .po files in the Creative Commons format, with abstract string
identifiers.


Installation
============

The toolset uses `zc.buildout <http://python.org/pypi/zc.buildout>`_ to
assemble the software and its dependencies. Buildout will download and install
any dependencies needed and install them in the local checkout directory.

To prepare the tools for use:

1. Clone this repository and change directory into it
2. Run the boostrap script::

    python bootstrap.py

3. Run the newly generated buildout script::

    ./bin/buildout

4. Create a virtual environment within the repository::

    virtualenv .

5. Activate the virtual environment::

    source bin/activate

6. Install the transifex client::

    pip install transifex-client

After the buildout process completes the tools will be available in a ``bin``
sub-directory. Note that the buildout process "bakes in" explicit paths to
any downloaded dependencies; if you move your installation to another location
on the filesystem, you must repeat the buildout process.


Managing Translations
=====================

Translations are managed through the ``master`` .po file and a set of tools.

To add or change translations, edit ``master/cc_org.po``. After editing
translations, run the sync script::

  ./bin/sync

This script will update all files in the ``po`` directory with changes to the
strings, as well as ``master/cc_org.po.bak.``  The .bak file is used to track
changes in the English text of strings. For this reason it is important that
the master files are committed along with the updated ``.po`` files.


Deploying Updated Translations
------------------------------

In order to deploy updated translations, the "normal" .po files need to be
converted to CC style files. This is handled by the po2cc script::

  ./bin/po2cc


Updating Translations
---------------------

1. Change directory into the repository
2. Ensure the repository is up-to-date::

    git pull

3. Activate the virtual environment::

    source bin/activate

4. Pull in new translations::

    tx pull -a --mode developer

5. Commit changes::

    git commit -a -m "Latest i18n updates from Transifex"

6. Push changes back to origin::

    git push origin master
