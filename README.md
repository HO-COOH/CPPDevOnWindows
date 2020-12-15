# The definitive guide of setting up C/C++ development environment on Windows
I know a lot of you are having troubles of getting it to work on Windows and complaining a shiton. 

You can just use [Visual Studio](https://visualstudio.microsoft.com/), which is the best and beginner-friendly solution, but for some reason you are just a boomer and don't want to use it. 

Now, you have found the right guide! **Follow the guide and screenshot carefully.** The screenshot are from Windows Sandbox, which is a clean install of Windows 10. **If you followed everything, and can't get it work, open an issue. Let me see how that's even possible!!** 

- [The definitive guide of setting up C/C++ development environment on Windows](#the-definitive-guide-of-setting-up-cc-development-environment-on-windows)
  - [Download these (Mandatory)](#download-these-mandatory)
  - [Install GCC](#install-gcc)
  - [Install CMake](#install-cmake)
  - [Setting up VSCode](#setting-up-vscode)
  - [Setting up CLion](#setting-up-clion)
  - [Install Git](#install-git)
  - [Setting up MSVC](#setting-up-msvc)
  - [Setting up vcpkg](#setting-up-vcpkg)
  - [Setting up WSL](#setting-up-wsl)
  - [Using a library](#using-a-library)
  - [Setting up doxygen](#setting-up-doxygen)
  - [Setting up a package manager](#setting-up-a-package-manager)
    - [Winget](#winget)
    - [Chocolatey](#chocolatey)

## Download these (Mandatory)
- [cmake](https://cmake.org/download/), choose the ``Windows win64-x64 Installer`` option
- [msys2](https://www.msys2.org/)

## Install GCC
1. Install MSYS2. Just launch the installer and keep clicking "Next"
2. Run MSYS2, type the following command:
```
pacman -Syu
```
It will update the packages info, so you get the latest packages. It will prompt you like this, and you type ``y`` and hit enter.
![](./screenshots/1.png)

3. Then it will prompt you `` To complete this update all MSYS2 processes including this terminal will be closed. Confirm to proceed [Y/n]``, type `y` and hit enter, and it will close the window after the update is done.
4. Relaunch MSYS2 from your start menu. Type:
```
pacman -S mingw-w64-x86_64-gcc
```
like this, type `y` and hit enter to install gcc
![](screenshots/2.png)

And then type:
```
pacman -S mingw-w64-x86_64-make
```
And type `y` to also install ``make``.

And then type:
```
pacman -S mingw-w64-x86_64-gdb
```
And type `y` to also install ``gdb``.

5. Now search for ``environment variable`` and open it
![](screenshots/3.png)

6. Click ``Environment Variables``, find ``Path`` in ``System variables``, double click to open the setting.
![](screenshots/4.png)

7. Click ``New`` and copy ``C:\msys64\mingw64\bin`` to the new entry.
![](screenshots/5.png)

8. Click ``OK`` to close all windows. Now you finished installing GCC.

## Install CMake
Launch the insatller, when you see this screen, choose ``Add CMake to the system PATH for all users``
![](screenshots/6.png)
Now you finished installing cmake.

## Setting up VSCode
1. Download [vscode](https://code.visualstudio.com/)
2. Launch the installer, when you see this screen, I **strongly recommend you follow this setting**
![](screenshots/7.png)

3. Run vscode, in the ``extension`` tab, search and install the following 3 extension
- This one is for C++ intellisense/syntax highlighting (or whatever)
![](screenshots/8.png)
- The first one in the list is for syntax highlighting when writing cmake scirpts.
- The second one in the list is for actually running Cmake.
![](screenshots/9.png)

4. Go to settings, search ``generator``. And set ``Cmake:Generator`` to ``MinGW Makefiles``, like this:
![](screenshots/12.png)

5. Create a folder, open it in vscode. Use ``ctrl + shift + p`` to open the command menu, type ``cmake`` and choose ``CMake: Quick Start``, like this:
![](screenshots/10.png)

6. The cmake tool will scan the kits and there will be 2 kits. Select the first one.
![](screenshots/11.png)

7. Type a name for your project, select ``Executable``, CMake tool will automatically generate a helloworld project for you. And you probably don't want to enable ctest for now, so delete everything excpet the following 3 lines:
![](screenshots/14.png)
Rememeber to click ``Allow`` when cmake want to configure the intellisense.

8. And now you can run it and debug it, and have everything working (syntax highlighting, auto complete, header files...).
![](screenshots/15.png)
![](screenshots/16.png) 

## Setting up CLion
1. Download [clion](https://www.jetbrains.com/clion/download/#section=windows)
2. Launch the installer, keep clicking "Next". When you see the following screen, I strongly recommend you to select ``Add "Open Folder as Project"``.
![](screenshots/17.png)

3. Run clion, set up the appearance as you like, login your account or free trial.
4. After those, it will prompt this window for setting up compilers, it should be all correct and no need to change.
![](screenshots/18.png)

5. Create a new C++ executable or C executable on the left
![](screenshots/20.png)

6. And everything should be working.
![](screenshots/19.png)


## Install Git
Most if not all of the development workflow involves using Git. Also, some [cmake](#install-cmake) functions requires Git to be installed. So you'd better install it [here](https://git-scm.com/download/win).

Git can be installed by keep clicking `Next`.![](screenshots/26.png)

## Setting up MSVC
I can understand you don't want to use Visual Studio because it's slow or for whatever reason. But you **HAVE TO** install Visual Studio on Windows to use `vcpkg`. (Mingw GCC **CAN NOT** be used to build `vcpkg` on Windows at the time being) And MSVC being a Microsoft C/C++ compiler is a legit and good compiler, and you do NOT need to use it with Visual Studio. You can also use it with [cmake](#install-cmake).
1. You need to download MSVC with Visual Studio [here](https://visualstudio.microsoft.com/downloads/). Choose the ``Community`` option.

2. Run the installer, select these workflows as the following
  ![](screenshots/21.png)

3. After installation, you will need to register a Microsoft Account to continue using Visual Studio. 

4. After Visual Studio is installed, cmake can detect it as a compiler.
  ![](screenshots/22.png)
  ![](screenshots/23.png)

## Setting up vcpkg
After [Visual Studio](#setting-up-msvc) is installed, you can use `vcpkg`.
``vcpkg`` is a C/C++ package manager, which makes using libraries much easier (almost as easy as using ``pip`` in python). Follow the guide [here](https://github.com/microsoft/vcpkg#quick-start-windows).

## Setting up WSL
Setting up WSL is the same as setting up a pure linux environment, therefore it is not discussed here. 

## Using a library
Of course you want to use [vcpkg](#setting-up-vcpkg). After you install the library in `vcpkg`, you either:
- Use `Visual Studio` without **ANY ADDITIONAL CONFIGURATION**
- Use `cmake` with the instruction provided by `vcpkg` when you install the library.

Below is a complete example of using `vcpkg` to install and use the [boost](https://www.boost.org/) library.

1. Install the library in `vcpkg` with the following command:
   ```
   vcpkg install boost:x64-windows
   ```
   Note that `vcpkg` will build 32 bit libraries by default, which is NOT probably what you want, so you want to speficy the architecture by adding `:x64-windows`.
   And you should see the following
   ![](screenshots/24.png)

2. Note that on Windows, `vcpkg` builds libraries using `MSVC`, so you should also use `MSVC` in order to link sucessfully. Header-only libraries like `boost` may be used with other compilers like `GCC`.
  - Use it in Visual Studio without doing any additional configuration
  ![](screenshots/25.png)
  **Note: Configure the solution achitectural target correctly according to your library. Visual Studio empty project defaults to `x86` but you may installed `x64` library.**

  - Or use it in VSCode/CLion with cmake and cmake tool chain file. See the docs [here](https://github.com/microsoft/vcpkg#using-vcpkg-with-cmake)

## Setting up doxygen
Writing good documentation is also an essential part of development. The most commonly used documentation generator is [doxygen](https://www.doxygen.nl/download.html). Download `the binary distribution for Windows` and then install it. After it is installed, there will be a GUI frontend called `doxywizard`, which looks like this:
![](screenshots/27.png)
To write good documentation, install these plugins:
- For [VSCode](https://marketplace.visualstudio.com/items?itemName=cschlosser.doxdocgen)
- For [Visual Studio](https://marketplace.visualstudio.com/items?itemName=FinnGegenmantel.doxygenComments)

Learn the syntax for documentation [here](https://www.doxygen.nl/index.html)

After you documment your code, any decent IDEs/text editors should be able to show the documentation, helping you better understand your own code as well as others.
![](screenshots/29.png)
![](screenshots/30.png)

Using doxygen is straight-forward using the GUI, just specify the root directory of your project, configure some settings to your liking, then run it.
![](screenshots/28.png)

Doxygen generated documentation too ugly? Follow the guide [here](https://devblogs.microsoft.com/cppblog/clear-functional-c-documentation-with-sphinx-breathe-doxygen-cmake/) to use doxygen with sphinx for a more beautiful documentation.

## Setting up a package manager
You thought Windows does not have an easy-to-use package manager? You might be wrong.

### Winget

### Chocolatey
