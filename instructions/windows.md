# Windows Instructions

some libraries could not be build with one simple conan command, so we need to build them manually.

## Mariadb

        curl -LO https://gitlab.com/terranum-conan/mariadb/-/archive/main/mariadb-main.tar.gz
        tar -zxvf mariadb-main.tar.gz
        cd mariadb-main
        conan source . --source-folder=_bin/source
        conan install . --install-folder=_bin/build --build=missing -s build_type=[Debug|Release]
        conan build . --source-folder=_bin/source --build-folder=_bin/build
        conan package . --source-folder=_bin/source --build-folder=_bin/build --package-folder=_bin/package
        conan export-pkg . terranum-conan+mariadb/stable --source-folder=_bin/source --build-folder=_bin/build
        conan test test_package mariadb/10.6.22@terranum-conan+mariadb/stable


## wxWidgets

        curl -LO https://gitlab.com/terranum-conan/wxwidgets/-/archive/main/wxwidgets-main.tar.gz
        tar -zxvf wxwidgets-main.tar.gz
        cd wxwidgets-main
        
        conan source . --source-folder=_bin
        conan install . --install-folder=_bin --build=missing -s build_type=[Debug|Release]
        conan build . --source-folder=_bin --build-folder=_bin
        conan export-pkg . terranum-conan+wxwidgets/stable --source-folder=_bin --build-folder=_bin
        conan test test_package wxwidgets/3.3.0@terranum-conan+wxwidgets/stable
