.. contents:: Table of Contents


Introduction
============

Plone extension providing smart logo handling.
Based on an svg Logo or Icon the extension is able to produce all sorts
of scales such as apple touch icons or android PWA logos.

Compatibility
-------------

Plone 4.3.x

Prerequisite
============

See `wand.py dependencies <http://docs.wand-py.org/en/0.4.4/index.html#requirements>`_

Installation
============

- Add the package to your buildout configuration:

::

    [instance]
    eggs +=
        ...
        ftw.logo


Development
===========

1. Fork this repo
2. Clone your fork
3. Shell: ``ln -s development.cfg buildout.cfg``
4. Shell: ``python bootstrap.py``
5. Shell: ``bin/buildout``

Run ``bin/test`` to test your changes.

Or start an instance by running ``bin/instance fg``.

Scales
======

Basically there are just logo and icon scales.
The logo scales are mostly used on the top right and can have
any dimensions. The converter creates a ``logo`` and ``mobile_logo`` out of the
base logo which has to be an svg file.
The icon scales are used for ``apple_touch_icons``, ``favicons`` or ``andoird PWA icons``.
The icons are square as well as the source.
That is becuase we need two different source files. One with an arbitary ratio
and the other with a square ratio.

All scales are taken from https://realfavicongenerator.net/.

The available scales are:

- LOGOS
   - LOGO
   - MOBILE_LOGO
   - BASE
- ICONS
   - APPLE_TOUCH_ICON
   - FAVICON_32X32
   - FAVICON_16X16
   - MSTILE_150X150
   - ANDROID_192X192
   - ANDROID_512X512
   - FAVICON
   - BASE

Converter
=========

The converter holds all different scale definitions so he is able to generate
the scales needed. `wand.py <http://docs.wand-py.org/en/0.4.4/>`_ is used
to convert the svg source files into the different scales.
The converter generates a modified ``wand.py``
image proxy which is able to return the actual blob of the scale behind the proxy.
Refer to the `write images <http://docs.wand-py.org/en/0.4.4/guide/write.html>`_ and
`resizing and cropping <http://docs.wand-py.org/en/0.4.4/guide/resizecrop.html>`_
section for more information about how the converter uses ``wand.py``.

ZCML
====

The extension introduces a custom icon and logo directive for zcml.
Both directives accept ``for``, ``layer`` and ``base`` attributes.
The base attribute defines the svg source files for all scales.
The multiadapter adapts context and request. So using ``for`` and ``layer`` the
base value can be overridden.

Example:

.. code-block:: xml

   <logo:logo base="logo.svg" />

The next block will override the previous config.

.. code-block:: xml

   <logo:logo base="custom_logo.svg" layer="IDummyLayer" />

Logo View
=========

All logos and icons can be access through the logo browser view.
The URL consists of the browser view name ``@@logo`` followed by the type of the
image and the actual scale.

Examples:

- ``@@logo/logo/BASE`` this will give the svg logo source.
- ``@@logo/icon/APPLE_TOUCH_ICON`` this will give apple touch icon as a png image.

Caching
=======

Caching is provided by adding a query string parameter to every logo request.
The cachekey consist of a sha256 hash including the files binary data.
If you have plone.app.caching enabled, install the `caching` profile from ftw.logo.
This will define etag values so the viewlet is cached properly.

Links
=====

- Github: https://github.com/4teamwork/ftw.logo
- Issues: https://github.com/4teamwork/ftw.logo/issues
- Pypi: http://pypi.python.org/pypi/ftw.logo


Copyright
=========

This package is copyright by `4teamwork <http://www.4teamwork.ch/>`_.

``ftw.logo`` is licensed under GNU General Public License, version 2.
