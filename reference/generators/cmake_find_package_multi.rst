.. _cmake_find_package_multi_generator_reference:


`cmake_find_package_multi`
==========================

.. warning::

    This is an **experimental** feature subject to breaking changes in future releases.

.. container:: out_reference_box

    This is the reference page for ``cmake_find_package_multi`` generator.
    Go to :ref:`Integrations/CMake<cmake>` if you want to learn how to integrate your project or recipes with CMake.


Generated files
---------------

For each conan package in your graph, it will generate 2 files and 1 more per different ``build_type``.
Being {name} the package name:

+------------------------------------+--------------------------------------------------------------------------------------+
| NAME                               | CONTENTS                                                                             |
+====================================+======================================================================================+
| {name}Config.cmake                 | It includes the {name}Targets.cmake and call find_dependency for each dep            |
+------------------------------------+--------------------------------------------------------------------------------------+
| {name}Targets.cmake                | It includes the following files                                                      |
+------------------------------------+--------------------------------------------------------------------------------------+
| {name}Targets-debug.cmake          | Specific information for the Debug configuration                                     |
+------------------------------------+--------------------------------------------------------------------------------------+
| {name}Targets-release.cmake        | Specific information for the Release configuration                                   |
+------------------------------------+--------------------------------------------------------------------------------------+
| {name}Targets-relwithdebinfo.cmake | Specific information for the RelWithDebInfo configuration                            |
+------------------------------------+--------------------------------------------------------------------------------------+
| {name}Targets-minsizerel.cmake     | Specific information for the MinSizeRel configuration                                |
+------------------------------------+--------------------------------------------------------------------------------------+

Targets
-------

A target named ``{name}::{name}`` target is generated with the following properties adjusted:

- ``INTERFACE_INCLUDE_DIRECTORIES``: Containing all the include directories of the package.
- ``INTERFACE_LINK_LIBRARIES``: Library paths to link.
- ``INTERFACE_COMPILE_DEFINITIONS``: Definitions of the library.

The targets contains multi-configuration properties, for example, the compile options property
is declared like this:

.. code-block:: cmake

        set_property(TARGET {name}::{name}
                 PROPERTY INTERFACE_COMPILE_OPTIONS
                     $<$<CONFIG:Release>:${{{name}_COMPILE_OPTIONS_RELEASE_LIST}}>
                     $<$<CONFIG:RelWithDebInfo>:${{{name}_COMPILE_OPTIONS_RELWITHDEBINFO_LIST}}>
                     $<$<CONFIG:MinSizeRel>:${{{name}_COMPILE_OPTIONS_MINSIZEREL_LIST}}>
                     $<$<CONFIG:Debug>:${{{name}_COMPILE_OPTIONS_DEBUG_LIST}}>)

The targets are also transitive. So, if your project depends on a packages ``A`` and ``B``, and at the same time
``A`` depends on ``C``, the ``A`` target will contain automatically the properties of the ``C`` dependency, so
in your `CMakeLists.txt` file you only need to ``find_package(A)`` and ``find_package(B)``.

You also need to adjust `CMAKE_PREFIX_PATH <https://cmake.org/cmake/help/v3.0/variable/CMAKE_PREFIX_PATH.html>`_ and
`CMAKE_MODULE_PATH <https://cmake.org/cmake/help/v3.0/variable/CMAKE_MODULE_PATH.html>`_ so CMake can locate all
the ``{name}Config.cmake`` files: The ``CMAKE_PREFIX_PATH`` is used by the ``find_package`` and the ``CMAKE_MODULE_PATH`` is used by the
``find_dependency`` calls that locates the transitive dependencies.
