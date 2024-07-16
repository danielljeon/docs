# STM32 CLion & STM32CubeMX Developer Environment Setup

The following documentation details how to set up JetBrains CLion for embedded
STM32 development utilizing STM32CubeMX, GNU ARM Toolchain and OpenOCD.

> Currently, the debugger on macOS when using this setup does not work. Build
> and Run have had no documented issues.

---

<details markdown="1">
  <summary>Table of Contents</summary>

- [1 Initial Software Installs / Setup](#1-initial-software-installs--setup)
    - [1.1 STM32CubeMX*](#11-stm32cubemx)
    - [1.2 CLion*](#12-clion)
    - [1.3 GNU Compiler Collection (GCC) ARM Toolchain*](#13-gnu-compiler-collection-gcc-arm-toolchain)
    - [1.4 OpenOCD*](#14-openocd)
- [2 Complete IDE Setup](#2-complete-ide-setup)
    - [2.1 Enable Embedded Development Support CLion Plugin](#21-enable-embedded-development-support-clion-plugin)
    - [2.2 Setup OpenOCD and STM32CubeMX Paths](#22-setup-openocd-and-stm32cubemx-paths)
    - [2.3 Setup Arm GNU Toolchain GCC and G++](#23-setup-arm-gnu-toolchain-gcc-and-g)
    - [2.4 Create new CLion STM32CubeMX project](#24-create-new-clion-stm32cubemx-project)
        - [2.4.1 Configure Version Control](#241-configure-version-control)
        - [2.4.2 Remove Default Generated File Structure](#242-remove-default-generated-file-structure)
    - [2.5 Configure STM32 with STM32CubeMX](#25-configure-stm32-with-stm32cubemx)
        - [2.5.1 Configure STM32CubeMX Project Manager](#251-configure-stm32cubemx-project-manager)
        - [2.5.2 Generate Code](#252-generate-code)
    - [2.6 Configure OpenOCD](#26-configure-openocd)
        - [2.6.1 Pick Target Board Configuration File](#261-pick-target-board-configuration-file)
        - [2.6.2 Configure Run and Debug](#262-configure-run-and-debug)
        - [2.6.3 Verify Run and Debug Config](#263-verify-run-and-debug-config)
    - [2.7 Hello World](#27-hello-world)

</details>

---

## 1 Initial Software Installs / Setup

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

### 1.2 CLion*

[CLion](https://www.jetbrains.com/clion/download/).

- JetBrains's C and C++ IDE.
- Students can register for
  the [Free Educational Licenses](https://www.jetbrains.com/shop/eform/students).

### 1.3 GNU Compiler Collection (GCC) ARM Toolchain*

[GNU Compiler Collection (GCC) ARM Toolchain](https://developer.arm.com/Tools%20and%20Software/GNU%20Toolchain).

- Toolchain for development on Arm based systems, built off of GNU Compiler
  Collection (GCC) by [ARM Developer](https://developer.arm.com/).
- Also, available via Homebrew
  on [ARM Developer Version](https://formulae.brew.sh/cask/gcc-arm-embedded).
    - Full original GNU version can be found
      on [Original GCC](https://formulae.brew.sh/formula/arm-none-eabi-gcc).

### 1.4 OpenOCD*

[OpenOCD](https://openocd.org/).

- For
  Windows: [GNU Toolchains OpenOCD](https://gnutoolchains.com/arm-eabi/openocd/)
    - **Put the main directory for OpenOCD in your PC user's `Documents`
      directory!**
- For macOS: [Homebrew OpenOCD](https://formulae.brew.sh/formula/open-ocd).

---

## 2 Complete IDE Setup

> For reference, official JetBrains documentation:
> [STM32CubeMX projects](https://www.jetbrains.com/help/clion/2023.1/embedded-development.html).

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
    C:\Program Files\STMicroelectronics\STM32Cube\STM32CubeMX\STM32CubeMX.exe
    ```
- For macOS the default paths are:
    ```
    /opt/homebrew/Cellar/open-ocd/0.12.0/bin/openocd
    /Applications/STMicroelectronics/STM32CubeMX.app/Contents/Resources/STM32CubeMX
    ```

### 2.3 Setup Arm GNU Toolchain GCC and G++

Add the path for `Arm GNU Toolchain` (`arm-none-eabi-gcc`).

```Settings → Build, Execution, Deployment → Toolchains```
![CLion Build Execution Deployment Toolchains.png](pictures/stm32ide/CLion%20Build%20Execution%20Deployment%20Toolchains.png?raw=true "CLion Build Execution Deployment Toolchains.png")

- For Windows the default paths are:
    ```
    C:\Program Files (x86)\Arm GNU Toolchain arm-none-eabi\12.3 rel1\bin\arm-none-eabi-gcc.exe
    C:\Program Files (x86)\Arm GNU Toolchain arm-none-eabi\12.3 rel1\bin\arm-none-eabi-g++.exe
    ```
- For macOS the default paths (for homebrew original GCC or ARM Developer
  version) are:
    ```
    /opt/homebrew/Cellar/ArmGNUToolchain/12.3.rel1/bin/arm-none-eabi-gcc
    /opt/homebrew/Cellar/ArmGNUToolchain/12.3.rel1/bin/arm-none-eabi-g++
    ... or ...
    /Applications/ArmGNUToolchain/12.3.rel1/arm-none-eabi/bin/arm-none-eabi-gcc
    /Applications/ArmGNUToolchain/12.3.rel1/arm-none-eabi/bin/arm-none-eabi-g++
    ```
    - Note that for macOS you may see a warning
      stating `Your CMake run finished with errors more...`.
        - Ignore this error for now, (future TODO).

### 2.4 Create new CLion STM32CubeMX project

```File → New → Project → STM32CubeMX```
![CLion new STM32CubeMX project.png](pictures/stm32ide/CLion%20new%20STM32CubeMX%20project.png?raw=true "CLion new STM32CubeMX project.png")

- Use the appropriate path if you are opening or creating a version controlled
  project.

#### 2.4.1 Configure Version Control

Wait for STM32CubeMX to finish generating a default project.

If you are using version control, verify that any required files such as `.git`
are still in the project directory, sometimes STM32CubeMX may delete / overwrite
these files.

#### 2.4.2 Remove Default Generated File Structure

Delete all files in your project except those that you specifically know are
needed.

If you are new to the STM32 workflow follow the recommendations below:

1. Keep `CMakeLists.txt`, `CMakeLists_template.txt`,
   `**PROJECT_NAME_HERE**.ioc`.
2. Keep files required for version control such as `.git`.
3. Keep `.idea` (JetBrain's project structure file).

### 2.5 Configure STM32 with STM32CubeMX

If STM32CubeMX fails to open automatically you can find the newly
created `**PROJECT_NAME_HERE**.ioc` on the left-hand `Project` file structure
viewer.

- You can edit the `.ioc` at any later time, but you should at least select
  your target ST chip or board.
- By default, the target chip will be selected to `STM32F030F4Px`.

#### 2.5.1 Configure STM32CubeMX Project Manager

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

#### 2.5.2 Generate Code

Generate code with the... `GENERATE CODE` button.

- You may get a warning about overwriting the previous `.ioc`, allow and
  continue.

### 2.6 Configure OpenOCD

#### 2.6.1 Pick Target Board Configuration File

Set the OpenOCD board file via `Select Board Configuration File` popup.

![CLion Select Board Config File.png](pictures/stm32ide/CLion%20Select%20Board%20Config%20File.png)

- Select the chip or board you are targeting, the default should be correct.
- Upon returning to CLion this popup should open automatically, if not we can
  select the board file in the following steps manually.

#### 2.6.2 Configure Run and Debug

```Run / Debug Configurations → Edit Configurations...```
![CLion ioc configured.png](pictures/stm32ide/CLion%20ioc%20configured.png)

- Top right drop down.

- By default, CLion should configure the OpenOCD configuration, the dropdown
  will be called `OCD **PROJECT_NAME_HERE**`.
- If this does not happen the dropdown will be labeled `Add Configuration...`

- Your file structure should look identical to the picture above except for any
  project / ST chip names.

#### 2.6.3 Verify Run and Debug Config

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

### 2.7 Hello World

Use the project file structure view on the left-hand to see the source code.

Build, Run (flash ST) and Debug are shown in the top mid-to-right.

![CLion successful flash.png](pictures/stm32ide/CLion%20successful%20flash.png)
