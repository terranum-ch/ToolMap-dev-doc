# ToolMap 2017

## Compilation et installation sous Linux

La version de Linux utilisée est Ubuntu 16.04.2 LTS (Xenial)

## Installation des librairies 

### Prérequis

        sudo apt-get install git cmake

### wxWidgets

        sudo apt-get install libwxgtk3.0-dev
        sudo apt-get install libwxgtk-webview3.0-dev
        sudo apt-get install libwxgtk-media3.0-dev

### Librairies spatiales (GDAL / GEOS / PROJ)

        sudo add-apt-repository ppa:ubuntugis/ubuntugis-unstable
        sudo apt-get install gdal-bin libgdal-dev


### MySQL 5.6.36

Version 5.6.36. Visiblement il y a une petite erreur de compilation étant donné que l'on n'utilise pas le moteur PARTITION. Il faut donc modifier le fichier `sql_table.cc` aux alentours de la ligne 2024 afin d'ajouter les deux lignes débutant avec des +

        ++++ #ifdef WITH_PARTITION_STORAGE_ENGINE
        if (old_part_info)
        {
                lpt->table->file->set_part_info(old_part_info, false);
        }
        +++++ #endif

Ensuite on peut lancer la compilation avec

        sudo apt-get install libncurses5-dev
        cmake .
        -DCMAKE_INSTALL_PREFIX=/home/lucien/programmation/LIB/_LIBMYSQL/
        -DWITH_UNIT_TESTS:BOOL=OFF
        -DFEATURE_SET:STRING=small

## Compilation ToolMap

1. Ouvrir le répertoire de ToolMap dans VSCODE

1. Modifier le fichier de préférence `settings.json` et ajouter le code suivant dans les paramètres de l'espace utilisateur:

                "cmake.sourceDirectory": "${workspaceRoot}/build",
                "cmake.buildDirectory": "${workspaceRoot}/../bin",
                "cmake.generator": "Unix Makefiles",
                "cmake.configureArgs": [
                        "-DMYSQL_MAIN_DIR=/home/lucien/programmation/LIB/_LIBMYSQL",
                        "-DSEARCH_WXPDFDOCUMENT_PATH=/home/lucien/programmation/LIB/_WXPDF",
                        "-DSEARCH_GEOS=1",
                        "-DSEARCH_GDAL=1"
                ]

1. Pour les tests unitaires, rajouter les paramètres suivants :

                ,"-DUSE_UNITTEST=1",
                "-DUNIT_TESTING_PATH=/home/lucien/programmation/ToolMap/test_data",
                "-DCXXTEST_INCLUDE_DIR=/home/lucien/programmation/LIB/cxxtest-4.4",
                "-DCXXTEST_PYTHON_TESTGEN_EXECUTABLE=/home/lucien/programmation/LIB/cxxtest-4.4/bin/cxxtestgen/cxxtestgen"                 