# LATEST BUILD INSTRUCTIONS #

## Conan

Install Conan, see https://docs.conan.io/en/latest/installation.html

Another option is to install pipx in order to easily deploy conan in a clean virtualenv. See https://pypa.github.io/pipx/
for more information

        python3 -m pip install --user pipx
        python3 -m pipx ensurepath
        pipx install conan==1.*  (conan 2.0 isn't supported)

## Toolmap

From the Toolmap directory create a build folder :

        mkdir cmake-build-release
        cd cmake-build-release

Install the required libraries with Conan

        conan install ..

Build using the following command

        conan build ..

## Build options

The following options are supported by `conan install ..` :

| Option                  | CMAKE equivalent         | Description                                                     |
|---------------------    |-------------------       |-----------------------------------------------------------------|
| -s build_type=Debug     | ---                      | Use the libraries and create the project in debug mode          |
| --build=missing         | ---                      | Build the missing libraries                                     |
| -o unit_test=True       | -DUSE_UNITTEST=ON        | Install the necessary libraries for the unit tests and run them |
| -o code_coverage=True   | -USE_CODECOV=ON          | Install the necessary libraries and options for code coverage   |
|                         | -USE_CODECOVERAGE_IDE=ON | Install options for code coverage into the IDE                  |

## Working from an IDE

If using an IDE, use the cmake and build from the IDE instead of the `conan build ..` command.

On OSX, run the following command  to fix the bundle with the correct dynamic library path

        cmake --install .

This step isn't needed when building using `conan build ..`.





