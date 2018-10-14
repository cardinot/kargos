kARGoS
=============

This is a fork of [ARGoS](https://www.argos-sim.info) with some changes to ease its setup for Kilobots.

Please, refer to the [main project](https://www.argos-sim.info) for more details.

## Requirements

If you downloaded the sources of ARGoS and want to compile its code, you need:

* A UNIX system (Linux or MacOSX; Microsoft Windows is not supported)
* _g++_ >= 4.3 (on Linux)
* _clang_ >= 3.1 (on MacOSX)
* _cmake_ >= 2.6

If you want to compile the simulator, you need:

* _FreeImage_ >= 3.15

The OpenGL-based graphical visualization is compiled only if the following
libraries are found:

* _Qt_ >= 5.5
* _freeglut_ >= 2.6.0
* _libxi-dev_ (on Ubuntu and other Debian-based systems)
* _libxmu-dev_ (on Ubuntu and other Debian-based systems)

If you want to create the Lua wrapper you need:

* _lua_ == 5.2

If you want to create the documentation you need:

* To create the API:
** _Doxygen_ >= 1.7.3
** _Graphviz/dot_ >= 2.28
* To create the HTML version of this README:
** _asciidoc_ >= 8.6.2

### Debian

On Debian, you can install all of the necessary requirements
with the following command:

``` bash
 $ sudo apt-get install cmake libfreeimage-dev libfreeimageplus-dev \
   qt5-default freeglut3-dev libxi-dev libxmu-dev liblua5.2-dev \
   lua5.2 doxygen graphviz graphviz-dev asciidoc
```

### OpenSuse

On openSUSE 13.2, you can install all of the necessary requirements
with the following commands:

``` bash
 $ sudo zypper ar -n openSUSE-13.2-Graphics \
   http://download.opensuse.org/repositories/graphics/openSUSE_13.2/ \
   graphics

 $ sudo zypper refresh

 $ sudo zypper install git cmake gcc gcc-c++ freeimage-devel \
   doxygen graphviz asciidoc lua-devel libqt5-qtbase freeglut-devel \
   rpmbuild
```

### Mac OSX

On Mac, you can install all of the necessary requirements using
http://http://brew.sh/[HomeBrew]. On the command line, type the
following command:

``` bash
 $ brew install pkg-config cmake libpng freeimage lua qt \
   docbook asciidoc graphviz doxygen
```

## Compiling the code

The compilation of ARGoS is configured through CMake.

``` bash
 $ cd argos3
 $ mkdir build_simulator
 $ cd build_simulator
 $ cmake ../src
 $ make
 ```

### Advanced compilation configuration

The compilation of ARGoS can be configured through a set of CMake options:

| Variable                 | Type      | Meaning [default value]
| :----------------------- | :-------- | :--------------------------
| +CMAKE_BUILD_TYPE+       | _STRING_  | Build type (+Debug+, +Release+, etc.) [Release]
| +CMAKE_INSTALL_PREFIX+   | _STRING_  | Install prefix (+/usr+, +/usr/local+, etc.) [+/usr+]
| +ARGOS_BUILD_FOR+        | _STRING_  | Target of compilation (+simulator+ or robot name) [+simulator+]
| +ARGOS_BUILD_NATIVE+     | _BOOLEAN_ | Whether to use platform-specific instructions [+OFF+]
| +ARGOS_THREADSAFE_LOG+   | _BOOLEAN_ | Use or not the thread-safe version of +LOG+/+LOGERR+. [+ON+]
| +ARGOS_DYNAMIC_LOADING+  | _BOOLEAN_ | Compile (and use) dynamic loading facilities [+ON+]
| +ARGOS_USE_DOUBLE+       | _BOOLEAN_ | Use +double+ (+ON+) or +float+ (+OFF+) [+ON+]
| +ARGOS_DOCUMENTATION+    | _BOOLEAN_ | Create API documentation [+OFF+]
| +ARGOS_INSTALL_LDSOCONF+ | _BOOLEAN_ | Install the file +/etc/ld.so.conf/argos3.conf+ [+ON+ on Linux, +OFF+ on Mac]

IMPORTANT: You can't install ARGoS system-wide and run the source version at the same time.
           If you intend to run ARGoS from the sources, you must uninstall it from the
           system.

### Running the ARGoS simulator

If you don't want to install ARGoS on your system, you can run it from the sources
tree. In the directory +build_simulator/+ you'll find a bash script called
+setup_env.sh+. Executing this script, you configure the current environment to
run ARGoS:

``` bash
 $ cd argos3
 $ cd build_simulator
 $ . setup_env.sh     # or 'source setup_env.sh'
 $ cd core
 $ ./argos3 -q all    # this shows all the plugins recognized by ARGoS
```

If you execute ARGoS with the graphical visualization, you'll notice that
icons and textures are missing. This is normal, as ARGoS by default looks
for them in the default install location. To fix this, you need to edit
the default settings of the GUI.

On Linux, edit the file +$HOME/.config/Iridia-ULB/ARGoS.conf+ as follows:

``` txt
 [MainWindow]
 #
 # other stuff
 #
 icon_dir=/PATH/TO/argos3/src/plugins/simulator/visualizations/qt-opengl/icons/
 texture_dir=/PATH/TO/argos3/src/plugins/simulator/visualizations/qt-opengl/textures/
 #
 # more stuff
 #
```

On Mac, write the following commands on the terminal window:

``` txt
 $ defaults write be.ac.ulb.Iridia.ARGoS MainWindow.texture_dir -string "/PATH/TO/argos3/src/plugins/simulator/visualizations/qt-opengl/textures/"
 $ defaults write be.ac.ulb.Iridia.ARGoS MainWindow.icon_dir -string "/PATH/TO/argos3/src/plugins/simulator/visualizations/qt-opengl/icons/"
 $ killall -u YOURUSERNAME cfprefsd
```
    
Be sure to substitute +/PATH/TO/+ with the correct path that contains the +argos3+
folder, and +YOURUSERNAME+ with your username as displayed on the terminal.


