# Linux Build

Our current recommanded and tested environment is **Ubuntu 16.04 LTS**. Theoratically you can build on other distros and OSX as well but we haven't tested it.

## Install and Build
It's super simple 1-2-3!

1. Make sure you are [registered with Epic Games](https://docs.unrealengine.com/latest/INT/Platforms/Linux/BeginnerLinuxDeveloper/SettingUpAnUnrealWorkflow/1/index.html). This is required so you can get Unreal engine's source code.
2. Clone Unreal in your favorite folder and run setup.sh (this may take a while!)
```
     mkdir -p GitHubSrc && cd GitHubSrc
     git clone -b 4.15 https://github.com/EpicGames/UnrealEngine.git
     cd UnrealEngine
     ./Setup.sh
     ./GenerateProjectFiles.sh
     make
```
3. Clone AirSim and run setup.sh:
```
     git clone https://github.com/Microsoft/AirSim.git
     cd AirSim
     ./setup.sh
     ./build.sh
```

## Running AirSim on Linux
Go to Unreal folder and bring up Unreal Editor:
```
cd Unreal/Engine/Binaries/Linux
UE4Editor
```

On first start you might not see any projects in UE4 editor. Click on Projects tab, Browse button and then select `AirSim/Unreal/Environments/Blocks/Blocks.uproject`. You will be then prompted by message "The following modules are missing or built with a different engine versions...". Click Yes. Now it might take a while so go get some coffee :).

## Changing Code and Rebuilding
1. After making code changes in AirSim, run `./build.sh` to rebuild. This step also copies the binary output to Blocks sample project. To clean and completely rebuild, first use `./clean.sh`.
2. Start UE4Editor as described in previous section and double click on Blocks project. This will rebuild Unreal binaries as well.

## Using AirSim Your Own Unreal Project
To setup your own Unreal environment [see these instructions](unreal_custenv,md).

To update your project with AirSim, simply copy the `AirSim/Unreal/Plugins` folder in to your project. You can use this handy command line:
```
rsync -a --delete Unreal/Plugins path/to/MyUnrealProject
```
You can also copy `clean.sh` from `AirSim/Unreal/Environments/Blocks` folder to your Unreal project folder.

## FAQ

#### Unreal crashed! How do I know what went wrong?
First go to folder `MyUnrealProject/Saved/Crashes` and then search directories for MyProject.log file. At the end of this file you will see stack trace and message. Also see `Diagnostics.txt` file.

#### How do I use IDE in Linux?
You can use Qt or CodeLite. Instructions for Qt Creator is [available here](https://docs.unrealengine.com/latest/INT/Platforms/Linux/BeginnerLinuxDeveloper/SettingUpAnIDE/index.html).

#### Can I cross compile for Linux from Windows machine?
Yes, you can but we haven't tested it. You can find [instructions here](https://docs.unrealengine.com/latest/INT/Platforms/Linux/GettingStarted/index.html).

#### What compiler and stdlib AirSim uses?
We use same compiler that Unreal uses which is Clang 3.9 for Unreal 4.15 nd Clang 4.0 for Unreal 4.16. AirSim's `setup.sh` will automatically download Clang 3.9. We also need to use libc++ that Unreal uses. The libc++ code is cloned by AirSim's `setup.sh` in to `llvm-source` folder and built in `llvm-build` folder. The cmake is the instructed to use libc++ from `llvm-build` folder. For other flavors of Linux and more info, please see [http://apt.llvm.org/](http://apt.llvm.org/).

#### Can use AirSim with Unreal 4.16?
Yes! The `*.Build.cs` files are, however, no longer compatible (you will get compile error). You can find files for 4.16 as `*.Build.4.16.cs` so just rename those. 

#### What CMake version AirSim build uses?
3.5.0 or higher. This should be default in Ubuntu 16.04. You can check your cmake version using `cmake --version`. If you have older version the follow [these instructions](cmake.md) or [see cmake website](https://cmake.org/install/).

#### Can I compile AirSim in BashOnWindows?
Yes, however you can't run Unreal from BashOnWindows. So this is kind of useful to check Linux compile, not for end-to-end run. See [BashOnWindows install guide](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide). Make sure to have latest version (Windows 10 Creators Edition) as previous versions had various issues. Also don't invoke `bash` from `Visual Studio Command Prompt` otherwise cmake might find VC++ and try and use that!

#### Where can I find more info on running Unreal on Linux?
* [Start here - Unreal on Linux](https://docs.unrealengine.com/latest/INT/Platforms/Linux/index.html)
* [Building Unreal on Linux](https://wiki.unrealengine.com/Building_On_Linux#Clang)
* [Unreal Linux Support](https://wiki.unrealengine.com/Linux_Support)
* [Unreal Cross Compilation](https://wiki.unrealengine.com/Compiling_For_Linux)

