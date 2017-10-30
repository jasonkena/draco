Description
===========

This folder contain resources for using Draco within Unity development.
Currently we support two types of usages:
* Import Draco compressed mesh as assets during design time.
* Load/decode Draco files in runtime.

Prerequisite
============

To start, you need to have the Draco unity plugin. You can either use the
prebuilt libraries provided in this folder or build from source.
Note that the plugin library for different platforms has different file extension.

| Platform | Library name |
| -------- | ------------ |
| Mac OS | dracodec_unity.bundle |
| Android | libdracodec_unity.so |
| Windows | TODO libdracodec_unity.dll |

Prebuilt Library
----------------

We have built library for several platforms:

| Platform | Tested Environment |
| -------- | ------------------ |
| .bundle  | macOS Sierra + Xcode 8.3.3 |
| armeabi-v7a(.so) | Android 8.1.0 |
| .dll | Win10 + Visual Studio 2017 |

Build From Source
-----------------
You can build the plugins on your own for osx/Win/Android. Source code for the wrapper is here: [src/draco/unity/](../src/draco/unity). Following is detailed building instruction.

Mac OS X
--------
On Mac OS X, run the following command to generate Xcode projects. It is the same as building Draco but with the addition of `-DBUILD_UNITY_PLUGIN=ON` flag:

~~~~~ bash
$ cmake path/to/draco -G Xcode -DBUILD_UNITY_PLUGIN=ON
~~~~~

Then open the project use Xcode and build.
You should be able to find the library under:
~~~~
path/to/build/Debug(or Release)/dracodec_unity.bundle
~~~~

Android
-------

You should first follow the [Android Studio Project Integration](../README.md#android-studio-project-integration) to build Draco within an Android project. Then, to build the plugin for Unity, just add the following flag to build.gradle.
~~~~
cppFlags "-DBUILD_UNITY_PLUGIN"
~~~~

You should be able to find the plugin library under:
~~~~
path/to/your/project/build/intermediates/cmake/debug(or release)/obj/your_platform/libdracodec_unity.so
~~~~

Windows
-------


Change file extension
---------------------
Because Unity can not recognize un-native file extension, you need to change your compressed .drc file to .drc.bytes so that Unity will recognize it as binary file. For example, if you have file `bunny.drc` then change the file name to `bunny.drc.bytes`. 

Copy Library to Your Project
----------------------------
Copy the plugin library to your Unity project in `Assets/Plugins/`.
For Android:
~~~~
cp path/to/your/libdracodec_unity.so path/to/your/Unity/Project/Assets/Plugins/Android/
~~~~
For Mac:
~~~~
cp path/to/your/dracodec_unity.bundle path/to/your/Unity/Project/Assets/Plugins/
~~~~
For Win:
~~~~
~~~~


Copy Unity Script to Your Project
---------------------------------
Copy the scripts in this folder to your project.
Copy wrapper:
~~~~
cp DracoMeshLoader.cs path/to/your/Unity/Project/Assets/
~~~~
Copy extened AssetPostProcessor which enables loading (This file is only used for import Draco files):
~~~~
cp DracoFileImporter.cs path/to/your/Unity/Project/Assets/Editor/
~~~~

Load Draco Assets in Runtime
============================
For example, please see [DracoDecodingObject.cs](DracoDecodingObject.cs) for usage. To start, you can create an empty GameObject and attach this script to it.

Enable Library in Script Debugging
----------------------------------
If you have library for the platform you are working on, e.g. `dracodec_unity.bundle` for Mac or `libdracodec_unity.dll` for  Windows. You should be able to use the plugin in debugging mode.

Import Compressed Draco Assets
==============================
In this section we will describe how to import Draco files (.drc) to Unity as
other 3D formats in design time, e.g. obj, fbx.
To note that importing Draco files doesn't mean the Unity project will export Draco files as models. It will not save space when exported to package.

If you have followed the previous steps, you just need to copy your asset, e.g. `bunny.drc.bytes`, to `Your/Unity/Project/Assets/Resources`, the project will automatically load the file and add the models to the project.

