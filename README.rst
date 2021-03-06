rTorrent-PS
===========

Extended `rTorrent`_ distribution with UI enhancements, colorization,
and some added features.

.. figure:: https://raw.githubusercontent.com/pyroscope/rtorrent-ps/master/docs/_static/img/rT-PS-094-2014-05-24-shadow.png
   :align: center
   :alt: Extended Canvas Screenshot

.. contents:: **Contents**


Introduction
------------

``rTorrent-PS`` is a `rTorrent`_ *distribution* (*not* a fork of it),
in form of a set of patches that improve the user experience and
stability of official ``rTorrent`` releases.

``rTorrent-PS`` is *not* the same as the ``PyroScope`` `command line
utilities <https://github.com/pyroscope/pyrocore#pyrocore>`_, and
doesn't depend on them; the same is true the other way 'round. It's just
that both unsurprisingly have synergies if used together, and some
features *do* only work when both are present.

To get in contact and share your experiences with other users of
``rTorrent-PS``, join the
`pyroscope-users <http://groups.google.com/group/pyroscope-users>`_
mailing list or the inofficial ``##rtorrent`` channel on
``irc.freenode.net``.


Feature Overview
----------------

The main changes compared to vanilla `rTorrent`_ are these:

-  self-contained install into any location of your choosing, including
   your home directory, offering the ability to run several versions at
   once (in different client instances).
-  rpath-linked to the major dependencies, so you can upgrade those
   independently from your OS distribution's versions.
-  extended command set:

   -  sort views by more than one value, and set the sort direction for
      each of these.
   -  bind keys in the root display to any command, e.g. change the
      built-in views.
   -  record network traffic.

-  interface additions:

   -  easily customizable colors.
   -  collapsed 1-line item display with condensed information.
   -  network bandwidth graph.
   -  displaying the tracker domain for each item.
   -  some more minor modifications to the download list view.

To get those, you just need to either follow the build instructions, or
download and install a package from Bintray — assuming one is available
for your platform. See below for installation instructions — more
detailed reference information can be found on the
`RtorrentExtended <https://github.com/pyroscope/rtorrent-ps/blob/master/docs/RtorrentExtended.md>`_
and
`RtorrentExtendedCanvas <https://github.com/pyroscope/rtorrent-ps/blob/master/docs/RtorrentExtendedCanvas.md>`_
pages.
`DebianInstallFromSource <https://github.com/pyroscope/rtorrent-ps/blob/master/docs/DebianInstallFromSource.md>`_
contains installation instructions for a working rTorrent instance in
combination with ``pyrocore``, on Debian and most Debian-derived distros
— i.e. a manual way to do parts of what
`pimp-my-box <https://github.com/pyroscope/pimp-my-box>`_ does
automatically for you.


Installation
------------

General Installation Options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

See the `instructions
here <https://github.com/pyroscope/rtorrent-ps/blob/master/docs/DebianInstallFromSource.md#build-rtorrent-and-core-dependencies-from-source>`_
for building from source using the provided ``build.sh`` script, which
will install *rTorrent-PS* into ``~/lib/rtorrent-‹version›``.

.. note:: If you also install the `PyroScope command line
    utilities <https://github.com/pyroscope/pyrocore>`_, do not forget to
    activate the extended features available together with *rTorrent-PS*, as
    mentioned in the
    `Configuration Guide <https://pyrocore.readthedocs.org/en/latest/setup.html#extending-your-rtorrent-rc>`_.

Also take note of the
`pimp-my-box <https://github.com/pyroscope/pimp-my-box>`_ project that
does it all (almost) automatically for Debian-type systems (and is the
preferred way to install on those systems). The automation is done using
`Ansible <http://docs.ansible.com/>`_, which implies you can easily
admin several systems with it, and also maintain them – so it's not a
one-shot installation bash script creating a setup that can never be
changed again.


Installation Using Debian Packages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For a limited set of Debian-derived platforms, there are packages
available that contain pre-compiled binaries (and only those, no
configuration or init scripts). You can download and install such a
package from `Bintray <https://bintray.com/pyroscope/rtorrent-ps>`_ —
assuming one is available for your platform. The packages install the
*rTorrent-PS* binary including some libraries into ``/opt/rtorrent``.

Example on Raspbian Jessie:

.. code-block:: bash

    version="0.9.6-20160308-c7c8d31~jessie_armhf"
    cd /tmp
    curl -Lko rt-ps.deb "https://bintray.com/artifact/download/pyroscope/rtorrent-ps/rtorrent-ps_$version.deb"
    dpkg -i rt-ps.deb

After installation, you must provide a configuration file
(``~/.rtorrent.rc``), and either use the absolute path to the binary to
start it, or link it into ``/usr/local`` like this:

.. code-block:: bash

    ln -s /opt/rtorrent/bin/rtorrent /usr/local/bin

.. note:: You can safely install the package and test it
    out in parallel to an existing installation, just use the absolute path
    ``/opt/rtorrent/bin/rtorrent`` to start rTorrent. Your data is in no way
    affected as long as you normally run a 0.9.x version.


Homebrew Tap for Mac OSX
~~~~~~~~~~~~~~~~~~~~~~~~

See the
`homebrew-rtorrent-ps <https://github.com/pyroscope/homebrew-rtorrent-ps>`_
repository for instructions to build *rTorrent-PS* and related
dependencies on Mac OSX.


Installation on Arch Linux
~~~~~~~~~~~~~~~~~~~~~~~~~~

There is an AUR package
`rtorrent-pyro-git <https://aur.archlinux.org/packages/rtorrent-pyro-git/>`_
for Arch Linux. If you have problems installing it, contact *the
maintainer* of the package.


Building the Debian Package
---------------------------

A Debian package for easy installation is built using
`fpm <https://github.com/jordansissel/fpm>`_, so you have to install
that first on the build machine, if you don't have it yet:

.. code-block:: bash

    apt-get install ruby ruby-dev
    gem install fpm
    fpm -h | grep fpm.version

Then you need to prepare the install target, as follows (we assume
building under the ``rtorrent`` user here):

.. code-block:: bash

    mkdir -p /opt/rtorrent
    chmod 0755 /opt/rtorrent
    chown -R rtorrent.rtorrent /opt/rtorrent

Then, the contents of the package are built by calling
``./build.sh install``, which will populate the ``/opt/rtorrent``
directory. When that is done, you can test the resulting executable
located at ``/opt/rtorrent/bin/rtorrent``.

Finally, ``./build.sh pkg2deb`` creates the Debian package in ``/tmp``.
The script expects the packager's name and email in the usual
environment variables, namely ``DEBFULLNAME`` and ``DEBEMAIL``. For a
few platforms (recent Debian, Ubuntu, and Raspbian), you can find
pre-built ones at
`Bintray <https://bintray.com/pyroscope/rtorrent-ps/rtorrent-ps>`_.


Building git HEAD of rTorrent
-----------------------------

You can also build the latest source of the main rTorrent project (including its ``libtorrent``),
with all the settings and rpath linking of the ``rtorrent-ps`` builds.

Start by checking out the two projects as siblings of the ``rtorrent-ps`` workdir.
Then use these commands to build them:

.. code-block:: bash

    ./build.sh clean_all
    ./build.sh all
    ./build.sh git

Just like with the vanilla and extended version, you'll get a ‘branded’ binary
called ``rtorrent-git``, and a symlink at ``~/bin/rtorrent`` will point to it.

Note however that the new ``libtorrent.so`` is unlikely to work with the
vanilla and extended code, so they'll be rendered unusable until you rebuild them.
Doing that will in turn render the git version broken.
This could be easily avoided if the (ABI) versions were bumped in git
directly after a release, but alas…


Trouble-Shooting
----------------

Startup Failure: ‘your terminal only supports 8 colors’
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Read these instructions:

-  `color
   configuration <https://github.com/pyroscope/rtorrent-ps/blob/master/docs/RtorrentExtended.md#uicolortypesetcolor-def>`_
-  `tmux and 256
   colors <https://github.com/pyroscope/rtorrent-ps/blob/master/docs/RtorrentExtendedCanvas.md#using-the-extended-canvas-with-tmux--screen-and-256-colors>`_
-  `(Windows) Terminal
   Setup <https://github.com/pyroscope/rtorrent-ps/blob/master/docs/RtorrentExtendedCanvas.md#setting-up-your-terminal>`_,
   and `Font Linking on
   Windows <https://github.com/chros73/rtorrent-ps_setup/wiki/Windows-8.1#font-linking-on-windows>`_

If all else fails, you can add a 
`configuration snippet <https://github.com/pyroscope/rtorrent-ps/blob/master/docs/examples/color_scheme8.rc>`_
to ``rtorrent.rc`` so that only 8 colors are used.


References
----------

-  https://github.com/rakshasa/rtorrent
-  `rTorrent Community Wiki <http://community.rutorrent.org/>`_

.. _`rTorrent`: https://github.com/rakshasa/rtorrent
