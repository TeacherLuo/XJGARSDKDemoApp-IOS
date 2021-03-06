# CMake Build Configuration for iOS

This is a blank single-view-controller iOS app, C++ dynamically linked framework and example tests which use CMake to create an Xcode-friendly out-of-source build system. Instead of configuring the build system through an `.xcodeproj` file, you instead maintain a set of `CMakeLists.txt` files describing the build system.

This CMake project can do everything Xcode can; eg build the executable app & the C++ library in `cppframework`. The build systems, generated into `build.ios` `build.sim` `build.sim64` `Xcode` are gitignored. CMakeLists.txt files are the only build configuration kept in source control. This is in contrast to committing the `.xcodeproj` directory which includes the backing XML, which is nonsensically hard to edit by hand.

The app instantiates a C++ object from the dynamically linked framework and calls a function on it. It subsequently deletes the C++ object pointer.

The test instantiates ObjC object of class `CppInterface` from the app and checks that it exists.

## What can this do for me?
- Conveniently use external C/C++/Objective-C libraries built with CMake in your app. Just clone the library and add an `add_subdirectory` to build that dependent library according to it's own build system, but directly as a target in your app's build system.
- Remove sections of code from your exectuable and instead load it just-in-time as a dynamically linked embedded framework.
    - You can decrease the size of your executable (and thus how much data must be loaded into memory when your app starts) by bundling subsections of the code up and packaging it as an embedded framework. This won't decrease your IPA size (since the framework must still be embedded) but it will make your executable smaller, and therefore quicker to start. If a user never runs the dynamically linked code, it stays out of memory. The sample framework code contains a C++ hello world but it could be C/C++/Objective-C.
- Stop dealing with and checking into source control pesky .xcodeproj and .xcworkspace directories. The scripts are designed to regenerate your build system frequently.


#### Create Xcode project for Devices (armv7, armv7s, arm64) and Simulators (i386, x86_64)
- Run `generate-project.sh` to create Xcode project in `Xcode`
- Run `open Xcode/project.xcodeproj`

### Framework
- Builds a dynamically linked iOS framework for the architectures relevant to the platform
- Sets the install name and rpath to the correct values for iOS packaging
- Copies the framework into the bundle of the app build as a post packaging step
- The same process can be done to any dynamically linked framework on disk
