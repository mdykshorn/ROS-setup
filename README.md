# ROS-setup
Guides for setting up ROS for easy development

### Background
It took me hours of trial and error to piece together the best way to use ROS in a development environment that allowed easy linting, code format and autocomplete. This guide aims to ease the process and combine resources to make setup easy.

Please contact me at mdd27@vt.edu for any suggestions or additions

### Environment
This guide will walk through setup in Atom, an open source code editor with many available packages.

Before settling on Atom, I tried:
* Sublime text
* VS code
* Eclipse

I never was able to get the environment setup how I would like in the other editors, so I am sticking to Atom for the time being.

More information on IDE's for ROS can be found [here](http://wiki.ros.org/IDEs)

### Prerequisites

* Ubuntu 16.04
* ROS kinetic (other ROS distros most likely work but are not tested)

## Installation

1. Install [atom](https://atom.io/)
2. Install clang `sudo apt install clang`
3. Install clang-format `sudo apt install clang-format-3.8`

## Setup

1. Launch Atom
2. open settings `ctrl + ,`
3. navigate to 'install package'
4. install the following packages
  1. [atom-ros](https://github.com/argenos/atom-ros)
  2. [linter](https://atom.io/packages/linter)
  3. [linter-clang](https://atom.io/packages/linter-clang)
  4. [autocomplete-clang](https://atom.io/packages/autocomplete-clang)
5. open the folder containing your workspace, you will have to create 2 files in this workspace to enable clang and clang-format as described below

#### .clang-format
@davetcoleman has a good repository containing the usage of the .clang_format for ROS

https://github.com/davetcoleman/roscpp_code_format

Most of the information is unnecessary for this guide, mainly you need to download the `.clang_format` file from his repository and put in the root of your workspace

#### .clang_complete
to allow clang find ROS includes and include files from your ROS package you will have to add a .clang_complete file at the root of your workspace

the clang_complete file should list the paths to each include directory you reference, at a minimum the following will be necessary, each on its own line
```
-I/opt/ros/kinetic/include
-Isrc/{your-package}/include
```
As an example my .clang_complete file contains the following for a package that uses custom messages, and thus needs the custom headers in the /devel directory. An example .clang_complete file is also included in this repository
```
-I/opt/ros/kinetic/include
-Isrc/pinpoint/include
-Isrc/devel/include
```
###### Key things to watch out for
* notice the lack of space between -I and the path
* notice the first '/' indicating direct path vs without indicating the path relative to the directory that contains the .clang_complete file
* if you cannot find include paths do the following:
  * run `catkin_make -DCMAKE_EXPORT_COMPILE_COMMANDS=ON`
  * navigate to the `/build` directory
  * there will be a file named `compile_commands.json`, inside this file is a listing of each include folder cmake generated
  * copy these includes to your `.clang_complete` file

## Building
Currently I have not setup `catkin_make` to run within Atom, I instead run it from a terminal

## Using the Environment
As a quick cheat sheet I've put common shortcuts here

*** as with most problems, if something isn't working, first restart Atom to see if that fixes the issue ***

autoformat code `super + shift + k`

lint code `ctrl + s` to save or `ctrl + shift + p` to bring up quick command palette and type `linter: lint`
