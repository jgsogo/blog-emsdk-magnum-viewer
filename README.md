# Example using Emscripten to build Magnum
Example using `magnum` to render 3D in the browser

### Build & Run locally

This example is using a recipe for `magnum` that is modified a little bit from the
one you can find in `conan-center-index` repository. These modifications are needed
in order to build it for Emscripten.

Steps to follow:

 1. (Optional) Configure Conan client

    ```
    export CONAN_USER_HOME=$(pwd)
    conan config set general.revisions_enabled=1
    ```

 1. Export modified `magnum` to local cache

    ```
    conan export .conan/recipes/magnum/all/conanfile.py magnum/2020.06@
    ```

 1. Create and move to your folder directory

    ```
    mkdir cmake-build-emsdk && cd cmake-build-emsdk
    ```

 1. Create the lockfile for your configuration

    ```
    conan lock create --profile:host=../.conan/profiles/emsdk --profile:build=default --lockfile=../lockfile.json --lockfile-out=lockfile.json --name=viewer --version=0.1 ../conanfile.txt --build --update
    ```
 
 1. Install and build missing packages (ConanCenter doesn´t provide binaries for Emscripten)

    ```
    conan install --lockfile=lockfile.json ../conanfile.txt --build=missing --generator=virtualenv
    ```

 1. Configure and build using CMake

    ```
    source activate.sh
    cmake .. -DCMAKE_MODULE_PATH=$(pwd) -DCMAKE_TOOLCHAIN_FILE=$CONAN_CMAKE_TOOLCHAIN_FILE
    make
    ```

 1. Run and enjoy

    ```
    python -m http.server --directory bin
    ```

    And open your browser at `http://localhost:8000/viewer.html`


### Update dependencies

In order to update the lockfile, just modify the `conanfile.txt` with new requirements
and then regenerate the base lockfile:

```
conan lock create --name=viewer --version=0.1 --base --lockfile-out lockfile.json conanfile.txt --build --profile:host=.conan/profiles/emsdk --profile:build=default
```
