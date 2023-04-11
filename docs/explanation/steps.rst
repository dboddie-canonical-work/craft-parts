.. _steps:

Steps
=====

Most pieces of software can be built using a sequence of separate, well-defined
steps. These describe how to obtain the source code and dependencies, overlay
them onto a base environment, build the components, stage the build artifacts for
curation, then prime them for distribution or further processing.

Craft Parts defines these steps as the `Pull`_, `Overlay`_, `Build`_, `Stage`_
and `Prime`_ steps. It runs them for all :ref:`parts` when preparing a payload.
These steps are run in sequence for a single part. For projects that define
multiple independent parts, the steps are run in sequence and the appropriate
actions are performed for all parts during each step.

However, because parts can depend on each other, the process of building them
may require some parts to be built and staged before others can be built.
This situation is explained further in the description of the
:ref:`parts lifecycle <lifecycle>`.

The mechanisms involved during each step is described in more detail in the
description of the :ref:`part build process <part_build_process>`.

Pull
----

In the pull step, the sources and dependencies for each part are obtained
using the appropriate `archive handlers <Craft Archives_>`_ for the types of
source specified. These are stored in a common cache directory for all parts in
a project.

Overlay
-------

In the overlay step, the sources and dependencies for each part are unpacked into
a base file system chosen from a collection of standard file system images.
Parts are unpacked into separate locations.

Build
-----

In the build step, each part with no unsatisfied build dependencies is built
using a plugin specified in the part's definition. The plugin is used to manage
the process of building the part for a specific language or build system.

If a part is required for the build step of another part, it will be staged
immediately after building. This enables parts to use the build products of other
parts in their build processes. See :ref:`lifecycle` for more information.

The plugin for a part installs build products and artifacts into a common
installation directory

Stage
-----

In the stage step, the build products or artifacts for each part are copied
into a common area. This allows conflicts between files provided by multiple
parts to be resolved before the final priming step is performed.
It also enables parts to be built for use by other parts in a project.

The selection of artifacts from a part's build step that are copied in the
staging area can be filtered with the use of the :ref:`stage` property.
This uses a :ref:`fileset <filesets_explanation>` to migrate files and
directories from a part's installation directory.

Prime
-----

In the prime step, files and directories in the common staging area are copied
into a common priming area for further processing.
Tools based on Craft Parts use the files in the priming area as the basis of the
packages and payloads that they produce.

.. include:: /links.txt
