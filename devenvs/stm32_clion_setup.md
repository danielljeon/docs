# STM32 CLion & STM32CubeMX Developer Environment Setup

The following documentation details how to set up JetBrains CLion for embedded
STM32 development utilizing STM32CubeMX, GNU ARM Toolchain and OpenOCD.

---

<details markdown="1">
  <summary>Table of Contents</summary>

- [1 Initial Software Installs](#1-initial-software-installs)
    - [1.1 STM32CubeMX*](#11-stm32cubemx)
    - [1.2 STM32CubeIDE*](#12-stm32cubeide)
    - [1.3 CLion*](#13-clion)
    - [1.4 GNU Compiler Collection (GCC) ARM Toolchain*](#14-gnu-compiler-collection-gcc-arm-toolchain)
    - [1.5 OpenOCD*](#15-openocd)
    - [1.6 STM32CubeProgrammer (Recommended)](#16-stm32cubeprogrammer-recommended)
- [2 CLion Embedded Development Setup](#2-clion-embedded-development-setup)
    - [2.1 Enable Embedded Development Support CLion Plugin](#21-enable-embedded-development-support-clion-plugin)
    - [2.2 Setup OpenOCD and STM32CubeMX Paths](#22-setup-openocd-and-stm32cubemx-paths)
    - [2.3 Setup Arm GNU Toolchain GCC and G++](#23-setup-arm-gnu-toolchain-gcc-and-g)
- [3 STM32CubeMX Project Setup](#3-stm32cubemx-project-setup)
    - [3.1 Create new CLion STM32CubeMX project](#31-create-new-clion-stm32cubemx-project)
        - [3.1.1 Configure Version Control](#311-configure-version-control)
        - [3.1.2 Remove Default Generated File Structure](#312-remove-default-generated-file-structure)
    - [3.2 Configure STM32 with STM32CubeMX](#32-configure-stm32-with-stm32cubemx)
        - [3.2.1 Configure STM32CubeMX Project Manager](#321-configure-stm32cubemx-project-manager)
        - [3.2.2 Generate Code](#322-generate-code)
    - [3.3 CMake Configuration](#33-cmake-configuration)
- [4 OpenOCD Configuration](#4-openocd-configuration)
    - [4.1 Pick Target Board Configuration File](#41-pick-target-board-configuration-file)
    - [4.2 Configure Run and Debug](#42-configure-run-and-debug)
        - [4.2.1 Verify Run and Debug Config](#421-verify-run-and-debug-config)
        - [4.2.2 Hello World](#422-hello-world)
- [5 Embedded GDB Server](#5-embedded-gdb-server)
- [6 Other Tools](#6-other-tools)

</details>

---

## 1 Initial Software Installs

### 1.1 STM32CubeMX*

[STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html).

- Graphical STM32 microcontroller configuration manager and code generator.

Prerequisites for macOS:

1. [Xcode](https://developer.apple.com/support/xcode/).
    - Homebrew Xcode packages work as well.

2. [Rosetta](https://support.apple.com/en-us/HT211861).

Installation for macOS:

1. Run `SetupSTM32CubeMX-6.9.1.app` for the application wizard.
2. If stopped by macOS, open the `Privacy & Security` in System Settings to
   allow running `SetupSTM32CubeMX-6.9.1.app`.

    - If wizard never opens or errors, try (replace `6.9.1` with your version):
    ```shell
    sudo xattr -cr ~/SetupSTM32CubeMX-6.9.1.app
    ```

### 1.2 STM32CubeIDE*

[STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html).

- Complete STM32 microcontroller IDE, used for in-CLion STLink debugger.
- Highly recommended if you are trying to debug and develop entirely in CLion.
- Without this OpenOCD debugger might work but certain firmware debug sessions
  have not acted correct for me during HAL setup (often clock config).

### 1.3 CLion*

[CLion](https://www.jetbrains.com/clion/download/).

- JetBrains's C and C++ IDE.
- Students can register for
  the [Free Educational Licenses](https://www.jetbrains.com/shop/eform/students).

### 1.4 GNU Compiler Collection (GCC) ARM Toolchain*

[GNU Compiler Collection (GCC) ARM Toolchain](https://developer.arm.com/Tools%20and%20Software/GNU%20Toolchain).

- Toolchain for development on Arm based systems, built off of GNU Compiler
  Collection (GCC) by [ARM Developer](https://developer.arm.com/).
- Also, available via Homebrew
  on [ARM Developer Version](https://formulae.brew.sh/cask/gcc-arm-embedded).
    - Full original GNU version can be found
      on [Original GCC](https://formulae.brew.sh/formula/arm-none-eabi-gcc).

**Recommended resource:**
The [Mbed Community Edition](https://github.com/mbed-ce/mbed-os) maintains a
well documented [GitHub wiki](https://github.com/mbed-ce/mbed-os/wiki) with
a [Toolchain Setup Guide](https://github.com/mbed-ce/mbed-os/wiki/Toolchain-Setup-Guide)
for ARM GCC.

### 1.5 OpenOCD*

[OpenOCD](https://openocd.org/).

- For
  Windows: [GNU Toolchains OpenOCD](https://gnutoolchains.com/arm-eabi/openocd/)
    - **Put the main directory for OpenOCD in your PC user's `Documents`
      directory!**
- For macOS: [Homebrew OpenOCD](https://formulae.brew.sh/formula/open-ocd).

### 1.6 STM32CubeProgrammer (Recommended)

[STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html).

- ST-LINK hardware application for bare-metal MCU memory operations.

---

## 2 CLion Embedded Development Setup

This section covers the initial setup for Embedded development on CLion.
Important steps such as ARM GCC executable file path declaration will be set up
here.

> For reference, official JetBrains documentation:
> [Embedded development](https://www.jetbrains.com/help/clion/embedded-overview.html).

### 2.1 Enable Embedded Development Support CLion Plugin

Add the `Embedded Development Support` plugin for CLion:
```Settings → Plugins```
![CLion Embedded Development Support plugin.png](pictures/stm32ide/CLion%20Embedded%20Development%20Support%20plugin.png)

### 2.2 Setup OpenOCD and STM32CubeMX Paths

Add the path for `OpenOCD` and `STM32CubeMX`:
```Settings → Build, Execution, Deployment → Embedded Development```
![CLion Embedded Development OpenOCD and STM32CubeMX path setting.png](pictures/stm32ide/CLion%20Embedded%20Development%20OpenOCD%20and%20STM32CubeMX%20path%20setting.png)

- You can always click the `Test` button to see if the path is valid.

- For Windows the default paths are:
    ```
    C:\Users\**NAME_HERE**\Documents\OpenOCD-20230712-0.12.0\bin\openocd.exe
    ```
    ```
    C:\Program Files\STMicroelectronics\STM32Cube\STM32CubeMX\STM32CubeMX.exe
    C:\Users\Daniel\AppData\Local\Programs\STM32CubeMX\STM32CubeMX.exe
    ```
- For macOS the default paths are:
    ```
    /opt/homebrew/Cellar/open-ocd/0.12.0/bin/openocd
    ```
    ```
    /Applications/STMicroelectronics/STM32CubeMX.app/Contents/Resources/STM32CubeMX
    ```

### 2.3 Setup Arm GNU Toolchain GCC and G++

Add the path for `Arm GNU Toolchain` (`arm-none-eabi-gcc`).

```Settings → Build, Execution, Deployment → Toolchains```
![CLion Build Execution Deployment Toolchains.png](pictures/stm32ide/CLion%20Build%20Execution%20Deployment%20Toolchains.png?raw=true "CLion Build Execution Deployment Toolchains.png")

- For Windows the default paths are:
    ```
    C:\Program Files (x86)\Arm GNU Toolchain arm-none-eabi\12.3 rel1\bin\arm-none-eabi-gcc.exe
    ```
    ```
    C:\Program Files (x86)\Arm GNU Toolchain arm-none-eabi\12.3 rel1\bin\arm-none-eabi-g++.exe
    ```
- For macOS the default paths (for homebrew original GCC or ARM Developer
  version) are:
    ```
    /opt/homebrew/Cellar/ArmGNUToolchain/12.3.rel1/bin/arm-none-eabi-gcc
    /Applications/ArmGNUToolchain/12.3.rel1/arm-none-eabi/bin/arm-none-eabi-gcc
    ```
    ```
    /opt/homebrew/Cellar/ArmGNUToolchain/12.3.rel1/bin/arm-none-eabi-g++
    /Applications/ArmGNUToolchain/12.3.rel1/arm-none-eabi/bin/arm-none-eabi-g++
    ```
    - Note that for macOS you may see a warning
      stating `Your CMake run finished with errors more...`.
        - Ignore this error for now, (future TODO).

---

## 3 STM32CubeMX Project Setup

CLion allows for integration with STM32CubeMX for pin configuration, code
generation and more.

> For reference, official JetBrains documentation:
> [STM32CubeMX projects](https://www.jetbrains.com/help/clion/embedded-development.html).

### 3.1 Create new CLion STM32CubeMX project

```File → New → Project → STM32CubeMX```
![CLion new STM32CubeMX project.png](pictures/stm32ide/CLion%20new%20STM32CubeMX%20project.png?raw=true "CLion new STM32CubeMX project.png")

- Use the appropriate path if you are opening or creating a version controlled
  project.

#### 3.1.1 Configure Version Control

Wait for STM32CubeMX to finish generating a default project.

If you are using version control, verify that any required files such as `.git`
are still in the project directory, sometimes STM32CubeMX may delete/overwrite
these files.

#### 3.1.2 Remove Default Generated File Structure

Delete all files in your project except those that you specifically know are
needed.

If you are new to the STM32 workflow follow the recommendations below:

1. Keep `CMakeLists.txt`, `CMakeLists_template.txt`,
   `**PROJECT_NAME_HERE**.ioc`.
2. Keep files required for version control such as `.git`.
3. Keep `.idea` (JetBrain's project structure file).

### 3.2 Configure STM32 with STM32CubeMX

If STM32CubeMX fails to open automatically you can find the newly
created `**PROJECT_NAME_HERE**.ioc` on the left-hand `Project` file structure
viewer.

- You can edit the `.ioc` at any later time, but you should at least select
  your target ST chip or board.
- By default, the target chip will be selected to `STM32F030F4Px`.

#### 3.2.1 Configure STM32CubeMX Project Manager

Configure the `Project Manager`.

1. `Project Name` = will be the name of your `.ioc`.
    - The recommended name is to follow the name of the (project) directory
      your `.ioc` is in.
2. `Project Location` = will be the root directory of your project.
3. `Toolchain / IDE` = `STM32CubeIDE`.
4. `Generate Under Root` = `True`.

![STM32CubeMX code generation settings.png](pictures/stm32ide/STM32CubeMX%20code%20generation%20settings.png)

- Verify that your paths are correct, notice here my `Project Location` is in
  the `GitHub` directory, not the repo directory.
    - My `Toolchain Folder Location` is the path of my repository since
      the `Project Name` matches the repo name.

#### 3.2.2 Generate Code

Generate code with the... `GENERATE CODE` button.

- You may get a warning about overwriting the previous `.ioc`, allow and
  continue.

### 3.3 CMake Configuration

Configurations for CMake will automatically trigger. If updates are possible,
the IDE will suggest through popups to reload from CMake.

**_Note: In CLion STM32 configuration `CMakeLists_template.txt` is used to
ensure consistent `CMakeLists.txt` creation following updates and code
generation in STM32CubeMX._** Feel free to edit `CMakeLists_template.txt` to
customize the linker process, such as adding additional required source code
directories.

---

## 4 OpenOCD Configuration

This section covers OpenOCD configuration for specific embedded targets. OpenOCD
will allow for running/flashing complied code, with (somewhat unstable)
debugging capabilities. _See following section(s) for a better debug approach._

> For reference, official JetBrains documentation:
> [OpenOCD support](https://www.jetbrains.com/help/clion/openocd-support.html).

### 4.1 Pick Target Board Configuration File

Continuing from STM32CubeMX `GENERATE CODE`, switch back to CLion.

Set the OpenOCD board file via `Select Board Configuration File` popup.

![CLion Select Board Config File.png](pictures/stm32ide/CLion%20Select%20Board%20Config%20File.png)

- Select the chip or board you are targeting, the default should be correct.
- **_Upon returning to CLion this popup should open automatically, if not we can
  select the board file in the following steps manually._**

### 4.2 Configure Run and Debug

```Run / Debug Configurations → Edit Configurations...```
![CLion ioc configured.png](pictures/stm32ide/CLion%20ioc%20configured.png)

- Top right drop down.

- By default, CLion should configure the OpenOCD configuration, the dropdown
  will be called `OCD **PROJECT_NAME_HERE**`.
- If this does not happen the dropdown will be labeled `Add Configuration...`

- Your file structure should look identical to the picture above except for any
  project/ST chip names.

#### 4.2.1 Verify Run and Debug Config

```Run / Debug Configurations → Edit Configurations...```

![CLion Edit Configurations.png](pictures/stm32ide/CLion%20Edit%20Configurations.png)

- If the OpenOCD configuration was not generated automatically, click
  the `+` button in the top left and `OpenOCD Download & Run`, then fill in
  the missing details shown below.
- `Target` = `**PROJECT_NAME_HERE**.elf`.
- `Executable binary` = `**PROJECT_NAME_HERE**.elf`.
- `Debugger` = `Bundled GDB`.
- `Board config file` = `**ST_TARGET_HERE_BOARD_FILE**.cfg`.
    - If you did not select a OpenOCD board file previously you can select one
      manually now.
        - Click `Assist...` and a popup with all board files should show up,
          select your target chip or board.

- You can also set the download and reset behaviour here.

### 4.2.2 Hello World

Use the project file structure view on the left-hand to see the source code.

Build, Run (flash ST) and Debug are shown in the top mid-to-right.

![CLion successful flash.png](pictures/stm32ide/CLion%20successful%20flash.png)

---

## 5 Embedded GDB Server

To set up in-IDE debugging, proper integration with debugger hardware such JLink
or STLink will be required. Follow JetBrains's documentation.

> For reference, official JetBrains documentation:
> [Embedded GDB Server](https://www.jetbrains.com/help/clion/embedded-gdb-server.html).

---

## 6 Other Tools

> For reference, official JetBrains documentation:
> [Peripheral view](https://www.jetbrains.com/help/clion/peripheral-view.html).
