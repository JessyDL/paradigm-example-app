# Description
This app shows the basic usage, and a quick intro in how to set a project up that uses [Paradigm Engine](https://github.com/JessyDL/paradigm) as a library.
Most code is in the `main.cpp` file, which goes through the steps of
- creating a surface to render on.
- loading an existing meta::library (to handle loading disk based assets, and tracking new ones)
- creation of a resource::cache (vital for asset management, and storing)
- `memory::region`: to allocate a region of memory to be used
- allocate GPU buffers (to store geometry, staging, etc..)
- loading shaders & textures
- generating a mesh dynamically (box), and material.

The project also comes with an example camera, and a showcase on how to use the `system::input` to listen to keyboard and mouse events.
# Basic Usage
## Building
Invoking the `build.sh` will require you to use the `bash on ubuntu on windows` environment on windows, though it is quite easy to replicate the helpers the build script sets up for you and just invoke `cmake` directly. 
From here on out, all platforms share the same setup.

**using `build.sh`**
Depending on how you installed the Vulkan SDK, the only thing you'll *need* to set is `VULKAN_VERSION`. If you have version 1.1.70.0 installed, then that will need to be invoked as `-VULKAN_VERSION="1.1.70.0"`. If for some reason the build script can't find the Vulkan SDK, you can set the root location (without the version in the path) using the `-VULKAN_ROOT="path/to/sdk"`.

You can set the cmake generator using the shorthand `-G`, which works identical to the cmake variant.

**using cmake directly**
It is perfectly possible to use cmake directly instead, as long as you set the following values.

`-DBUILD_DIRECTORY="path/to/where/to/build/to"` (not to be confused with where the project files will be)
`-DVULKAN_ROOT="path/to/sdk"` path to install location of the vulkan SDK (excluding the version part).
`-DVULKAN_VERSION="1.1.70.0"` string based value that is a 1-1 match with a installed vulkan SDK instance.

## Running
To successfully run the project, you will need to navigate to your build output directory, and navigate down to the bin folder, which you'll find for a default MSVC debug build in `builds/msvc/x64-VC++/debug/x64/bin`. You'll find the `example_game` executable there. You'll need to copy the contents of the [example data repository](https://github.com/JessyDL/paradigm-example-data) into that location, otherwise the game will ungraciously exit complaining about missing shaders. When all of that is done, you should be greeted with the following:

![enter image description here](https://raw.githubusercontent.com/JessyDL/paradigm-example-app/master/example_app_01.png)
# Extras
### Statically bind Vulkan
If you wish to statically bind vulkan, you can use the flag `-VULKAN_STATIC` in the `build.sh` script, or alternatively invoke `cmake` directly with the `-DVULKAN_STATIC=true` flag. 
Note that depending on the platform, static binding is impossible (like Android).
