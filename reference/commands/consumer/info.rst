
.. _conan_info:

conan info
==========

.. code-block:: bash

    $ conan info [-h] [--paths] [-bo BUILD_ORDER] [-g GRAPH]
                 [-if INSTALL_FOLDER] [-j [JSON]] [-n ONLY]
                 [--package-filter [PACKAGE_FILTER]] [-db [DRY_BUILD]]
                 [-b [BUILD]] [-r REMOTE] [-u] [-l LOCKFILE]
                 [--lockfile-out LOCKFILE_OUT] [-e ENV_HOST] [-e:b ENV_BUILD]
                 [-e:h ENV_HOST] [-o OPTIONS_HOST] [-o:b OPTIONS_BUILD]
                 [-o:h OPTIONS_HOST] [-pr PROFILE_HOST] [-pr:b PROFILE_BUILD]
                 [-pr:h PROFILE_HOST] [-s SETTINGS_HOST]
                 [-s:b SETTINGS_BUILD] [-s:h SETTINGS_HOST]
                 [-c CONF_HOST] [-c:b CONF_BUILD] [-c:h CONF_HOST]
                 path_or_reference

Gets information about the dependency graph of a recipe.

It can be used with a recipe or a reference for any existing package in
your local cache.

.. code-block:: text

    positional arguments:
      path_or_reference     Path to a folder containing a recipe (conanfile.py or
                            conanfile.txt) or to a recipe file. e.g.,
                            ./my_project/conanfile.txt. It could also be a
                            reference

    optional arguments:
      -h, --help            show this help message and exit
      --paths               Show package paths in local cache
      -bo BUILD_ORDER, --build-order BUILD_ORDER
                            given a modified reference, return an ordered list to
                            build (CI). [DEPRECATED: use 'conan lock build-order
                            ...' instead]
      -g GRAPH, --graph GRAPH
                            Creates file with project dependencies graph. It will
                            generate a DOT or HTML file depending on the filename
                            extension
      -if INSTALL_FOLDER, --install-folder INSTALL_FOLDER
                            local folder containing the conaninfo.txt and
                            conanbuildinfo.txt files (from a previous conan
                            install execution). Defaulted to current folder,
                            unless --profile, -s or -o is specified. If you
                            specify both install-folder and any setting/option it
                            will raise an error.
      -j [JSON], --json [JSON]
                            Path to a json file where the information will be
                            written
      -n ONLY, --only ONLY  Show only the specified fields: "id", "build_id",
                            "remote", "url", "license", "requires", "update",
                            "required", "date", "author", "description",
                            "provides", "deprecated", "None". '--paths'
                            information can also be filtered with options
                            "export_folder", "build_folder", "package_folder",
                            "source_folder". Use '--only None' to show only
                            references.
      --package-filter [PACKAGE_FILTER]
                            Print information only for packages that match the
                            filter pattern e.g., MyPackage/1.2@user/channel or
                            MyPackage*
      -db [DRY_BUILD], --dry-build [DRY_BUILD]
                            Apply the --build argument to output the information,
                            as it would be done by the install command
      -b [BUILD], --build [BUILD]
                            Given a build policy, return an ordered list of
                            packages that would be built from sources during the
                            install command
      -r REMOTE, --remote REMOTE
                            Look in the specified remote server
      -u, --update          Will check if updates of the dependencies exist in the
                            remotes (a new version that satisfies a version range,
                            a new revision or a newer recipe if not using
                            revisions).
      -l LOCKFILE, --lockfile LOCKFILE
                            Path to a lockfile
      --lockfile-out LOCKFILE_OUT
                            Filename of the updated lockfile
      -e ENV_HOST, --env ENV_HOST
                            Environment variables that will be set during the
                            package build (host machine). e.g.: -e
                            CXX=/usr/bin/clang++
      -e:b ENV_BUILD, --env:build ENV_BUILD
                            Environment variables that will be set during the
                            package build (build machine). e.g.: -e:b
                            CXX=/usr/bin/clang++
      -e:h ENV_HOST, --env:host ENV_HOST
                            Environment variables that will be set during the
                            package build (host machine). e.g.: -e:h
                            CXX=/usr/bin/clang++
      -o OPTIONS_HOST, --options OPTIONS_HOST
                            Define options values (host machine), e.g.: -o
                            Pkg:with_qt=true
      -o:b OPTIONS_BUILD, --options:build OPTIONS_BUILD
                            Define options values (build machine), e.g.: -o:b
                            Pkg:with_qt=true
      -o:h OPTIONS_HOST, --options:host OPTIONS_HOST
                            Define options values (host machine), e.g.: -o:h
                            Pkg:with_qt=true
      -pr PROFILE_HOST, --profile PROFILE_HOST
                            Apply the specified profile to the host machine
      -pr:b PROFILE_BUILD, --profile:build PROFILE_BUILD
                            Apply the specified profile to the build machine
      -pr:h PROFILE_HOST, --profile:host PROFILE_HOST
                            Apply the specified profile to the host machine
      -s SETTINGS_HOST, --settings SETTINGS_HOST
                            Settings to build the package, overwriting the
                            defaults (host machine). e.g.: -s compiler=gcc
      -s:b SETTINGS_BUILD, --settings:build SETTINGS_BUILD
                            Settings to build the package, overwriting the
                            defaults (build machine). e.g.: -s:b compiler=gcc
      -s:h SETTINGS_HOST, --settings:host SETTINGS_HOST
                            Settings to build the package, overwriting the
                            defaults (host machine). e.g.: -s:h compiler=gcc
      -c CONF_HOST, --conf CONF_HOST
                            Configuration to build the package, overwriting the defaults (host machine). e.g.: -c
                            tools.cmake.cmaketoolchain:generator=Xcode
      -c:b CONF_BUILD, --conf:build CONF_BUILD
                            Configuration to build the package, overwriting the defaults (build machine). e.g.: -c:b
                            tools.cmake.cmaketoolchain:generator=Xcode
      -c:h CONF_HOST, --conf:host CONF_HOST
                            Configuration to build the package, overwriting the defaults (host machine). e.g.: -c:h
                            tools.cmake.cmaketoolchain:generator=Xcode



**Examples**:

.. code-block:: bash

    $ conan info .
    $ conan info myproject_folder
    $ conan info myproject_folder/conanfile.py
    $ conan info hello/1.0@user/channel

The output will look like:

.. code-block:: bash

    Dependency/0.1@user/channel
     ID: 5ab84d6acfe1f23c4fae0ab88f26e3a396351ac9
     BuildID: None
     Context: host
     Remote: None
     URL: http://...
     License: MIT
     Description: A common dependency
     Updates: Version not checked
     Creation date: 2017-10-31 14:45:34
     Required by:
        hello/1.0@user/channel

    hello/1.0@user/channel
     ID: 5ab84d6acfe1f23c4fa5ab84d6acfe1f23c4fa8
     BuildID: None
     Context: host
     Remote: None
     URL: http://...
     License: MIT
     Description: Hello World!
     Updates: Version not checked
     Required by:
        Project
     Requires:
        hello0/0.1@user/channel

:command:`conan info` builds the complete dependency graph, like :command:`conan install` does. The main
difference is that it doesn't try to install or build the binaries, but the package recipes
will be retrieved from remotes if necessary.

It is very important to note, that the :command:`info` command outputs the dependency graph for a
given configuration (settings, options), as the dependency graph can be different for different
configurations. Then, the input to the :command:`conan info` command is the same as :command:`conan install`,
the configuration can be specified directly with settings and options, or using profiles.

Also, if you did a previous :command:`conan install` with a specific configuration, or maybe different
installs with different configurations, you can reuse that information with the :command:`--install-folder`
argument:

.. code-block:: bash

    $ # dir with a conanfile.txt
    $ mkdir build_release && cd build_release
    $ conan install .. --profile=gcc54release
    $ cd .. && mkdir build_debug && cd build_debug
    $ conan install .. --profile=gcc54debug
    $ cd ..
    $ conan info . --install-folder=build_release
    > info for the release dependency graph install
    $ conan info . --install-folder=build_debug
    > info for the debug dependency graph install


It is possible to use the :command:`conan info` command to extract useful information for Continuous
Integration systems. More precisely, it has the :command:`--build-order, -bo` option (deprecated in
favor of :ref:`conan lock build-order<versioning_lockfiles_build_order>`), that will produce
a machine-readable output with an ordered list of package references, in the order they should be
built. E.g., let's assume that we have a project that depends on Boost and Poco, which in turn
depends on OpenSSL and zlib transitively. So we can query our project with a reference that has
changed (most likely due to a git push on that package):

.. code-block:: bash

    $ conan info . -bo zlib/1.2.11@
    [zlib/1.2.11], [openssl/1.0.2u], [boost/1.71.0, poco/1.9.4]

Note the result is a list of lists. When there is more than one element in one of the lists, it means
that they are decoupled projects and they can be built in parallel by the CI system.

You can also specify the :command:`--build-order=ALL` argument, if you want just to compute the whole dependency graph build order

.. code-block:: bash

    $ conan info . --build-order=ALL
    > [zlib/1.2.11], [openssl/1.0.2u], [boost/1.71.0, poco/1.9.4]


Also you can get a list of nodes that would be built (simulation) in an install command specifying a build policy with the ``--build`` parameter.

E.g., if I try to install ``boost/1.71.0`` recipe with ``--build missing`` build policy and ``arch=x86``, which libraries will be built?

.. code-block:: bash

	$ conan info boost/1.71.0@ --build missing -s arch=x86
	bzip2/1.0.8, zlib/1.2.11, boost/1.71.0


You can generate a graph of your dependencies, in dot or html formats:

.. code-block:: bash

    $ conan info .. --graph=file.html
    $ file.html # or open the file, double-click

.. image:: /images/conan-info_deps_html_graph.png
    :height: 250 px
    :width: 300 px
    :align: center


The generated html output contains links to third party resources, the *vis.js* library (2 files: *vis.min.js*, *vis.min.css*).
By default they are retrieved from cloudfare. However, for environments without internet connection, these files
could be also used from the local cache and installed with :command:`conan config install` by putting those
files in the root of the configuration folder:

- *vis.min.js*: Default link to "https://cdnjs.cloudflare.com/ajax/libs/vis/4.18.1/vis.min.js"
- *vis.min.css*: Default link to "https://cdnjs.cloudflare.com/ajax/libs/vis/4.18.1/vis.min.css"

It is not necessary to modify the generated html file. Conan will automatically use the local paths to the cache files if
present, or the internet ones if not.

You can find where the package is installed in your cache by using the argument :command:`--paths`:

.. code-block:: bash

    $ conan info foobar/1.0.0@user/channel --paths

The output will look like:

.. code-block:: bash

    foobar/1.0.0@user/channel
        ID: 6af9cc7cb931c5ad942174fd7838eb655717c709
        BuildID: None
        Context: host
        export_folder: /home/conan/.conan/data/foobar/1.0.0/user/channel/export
        source_folder: /home/conan/.conan/data/foobar/1.0.0/user/channel/source
        build_folder: /home/conan/.conan/data/foobar/1.0.0/user/channel/build/6af9cc7cb931c5ad942174fd7838eb655717c709
        package_folder: /home/conan/.conan/data/foobar/1.0.0/user/channel/package/6af9cc7cb931c5ad942174fd7838eb655717c709
        Remote: None
        License: MIT
        Description: Foobar project
        Author: Dummy
        Topics: None
        Recipe: Cache
        Binary: Cache
        Binary remote: None
        Creation date: 2019-09-03 11:22:17
