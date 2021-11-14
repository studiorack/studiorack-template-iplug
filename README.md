# studiorack-template-iplug
![Release](https://github.com/studiorack/studiorack-template-iplug/workflows/Release/badge.svg)

Audio plugin starter template using iPlug framework to build binaries using:

* Bash
* CMake 3.4.x
* iPlug 2.0.x


## Installation

Install CMake and Xcode using:

    brew install cmake
    xcode-select --install

Check you have the correct dependencies installed:

    cmake -version
    xcodebuild -version

Ensure all git submodules are initialized:

    git submodule update --init --recursive

Install iPlug dependencies:

    cp -v ./src/CMakeLists.txt ./iplugsdk/Examples/IPlugInstrument
    cp -v ./src/download-iplug-sdks.sh ./iplugsdk/Dependencies/IPlug
    cp -v ./iplugsdk/Examples/IPlugInstrument/resources/fonts/Roboto-Regular.ttf ./iplugsdk/Examples/IPlugInstrument/resources
    cd ./iplugsdk/Dependencies/IPlug &&  ./download-iplug-sdks.sh && cd ../../..
    cd ./iplugsdk/Dependencies &&  ./download-prebuilt-libs.sh && cd ../..


## Usage

Make all your plugin development changes in the source folder at:

    ./src

Ensure you also update the preview image and audio files:

    ./src/assets/name.png
    ./src/assets/name.wav


## Testing your plugin

Todo


## Build (manual)

Depending on the the operating system you are on/building for, swap the generator string in the build commands:

* Linux: "Unix Makefiles"
* MacOS: "Xcode"
* Windows: "Visual Studio 16 2019"

Compile a development version of the plugin using:

    cmake \
      -G "Xcode" \
      -DCMAKE_BUILD_TYPE=Debug \
      -S ./iplugsdk/Examples/IPlugInstrument \
      -B ./build
    cmake --build ./build --config Debug --target VST3

View the built plugin files at:

    ./iplugsdk/Examples/IPlugInstrument/build

Build the final plugin binaries using:

    cmake \
      -G "Xcode" \
      -DCMAKE_BUILD_TYPE=Release \
      -S ./iplugsdk/Examples/IPlugInstrument \
      -B ./build
    cmake --build ./build --config Release --target VST3

Rename file if it contains invalid characters/spaces:

    mv "./build/Release/IPlugInstrument.vst3" "./build/Release/ipluginstrument.vst3"

Copy any additional files:

    cp -v ./src/assets/* ./build/Release
    echo -n "BNDL????" > ./build/Release/ipluginstrument.vst3/Contents/PkgInfo

For metadata generation as json use the studiorack-cli:

    npm install @studiorack/cli -g

Validate your plugin:

    studiorack validate "./build/Release/**/*.{vst,vst3}"

Convert and enrich validator report metadata into json:

    studiorack validate "./build/Release/**/*.{vst,vst3}" --json


## Build (automatic)

Release a plugin version on GitHub by simply creating a version tag:

    git tag v0.0.1
    git push && git push origin --tags

This will run an automated build and release process on GitHub Actions:

    .github/workflows/release.yml


## Resources & guides

* [iPlug framework source code and examples](https://github.com/iPlug2/iPlug2)
* [Official iPlug guide to creating VST audio plugins](https://github.com/iPlug2/iPlug2/wiki)


## Contact

For more information please contact kmturley
