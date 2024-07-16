# NXP S32K1xx MATLAB & Simulink Developer Environment Setup

The following documentation details how to set up MATLAB and Simulink for NXP
S32K1xx embedded development.

---

<details markdown="1">
  <summary>Table of Contents</summary>

- [1 Initial Software Installs / Setup](#1-initial-software-installs--setup)
    - [1.1 MATLAB* and Add-ons*](#11-matlab-and-add-ons)
- [2 Step-by-Step Setup](#2-step-by-step-setup)
    - [2.1 Opening NXP Support Package S32K1xx](#21-opening-nxp-support-package-s32k1xx)
        - [2.1.1 Finding the Add-on](#211-finding-the-add-on)
        - [2.1.2 Running the MATLAB Script](#212-running-the-matlab-script)
        - [2.1.3 Script GUI](#213-script-gui)
    - [2.2 S32Kxx Model-Based Design (MBT) Toolbox Install](#22-s32kxx-model-based-design-mbt-toolbox-install)
        - [2.2.1 Download](#221-download)
        - [2.2.2 Install](#222-install)
        - [2.2.3 Verify Install](#223-verify-install)
    - [2.3 S32Kxx Model-Based Design (MBT) Toolbox License](#23-s32kxx-model-based-design-mbt-toolbox-license)
        - [2.3.1 Obtain Host ID](#231-obtain-host-id)
            - [2.3.1.1 Windows](#2311-windows)
            - [2.3.1.2 macOS](#2312-macos)
            - [2.3.1.3 Linux](#2313-linux)
        - [2.3.2 Generate License File](#232-generate-license-file)
        - [2.3.3 Download License File](#233-download-license-file)
        - [2.3.4 Activate License File](#234-activate-license-file)
    - [2.4 Verify Complete Setup](#24-verify-complete-setup)
- [3 Initial Project](#3-initial-project)

</details>

---

## 1 Initial Software Installs / Setup

### 1.1 MATLAB* and Add-ons*

[MATLAB + Add-ons](https://www.mathworks.com/downloads/)* only for Windows.

- Some MATLAB packages are only supported for Windows:
    1. Simulink.
    2. Embedded Coder.
    3. Simulink Coder.
    4. Stateflow.
    5. NXP Support Package S32K1xx.

---

## 2 Step-by-Step Setup

### 2.1 Opening NXP Support Package S32K1xx

#### 2.1.1 Finding the Add-on

Open up MATLAB Add-Ons → Add-On Manager.

- Right-click on the previously installed `NXP Support Package S32K1xx` and
  select Open Folder.
  ![MATLAB Open NXP Support Package Files.png](pictures/nxp/MATLAB%20Open%20NXP%20Support%20Package%20Files.png)

#### 2.1.2 Running the MATLAB Script

Run the `NXP_Support_Package_S32K1xx.m` file.

- If you're new to MATLAB, it's under EDITOR → Run.
  ![MATLAB NXP Support Package Script.png](pictures/nxp/MATLAB%20NXP%20Support%20Package%20Script.png)

#### 2.1.3 Script GUI

The Support Package window should open.

![MATLAB NXP Opened install guide.png](pictures/nxp/MATLAB%20NXP%20Opened%20install%20guide.png)

- After completing each sub-step, the clickable buttons should turn green.
- The web page for both download `Files` and `License Keys`
  pages (`NXP Model-Based Design Toolbox for S32Kxx Automotive Microprocessors Family`)
  should look something like this:
  ![MATLAB NXP MBD Page.png](pictures/nxp/MATLAB%20NXP%20MBD%20Page.png)

### 2.2 S32Kxx Model-Based Design (MBT) Toolbox Install

#### 2.2.1 Download

Download the newest version of the MBT Toolbox.

- As of writing (`2024-01-28`), the window for setting up version `4.3.0`:

Use the recommend link options below (in order of highest to least recommended):

> You can use the newly opened window, but it and NXP's website is super buggy!

1. [NXP's Software Portal](https://www.nxp.com/webapp/swlicensing/sso/downloadSoftware.sp?catid=MCTB-EX)
    ```
    "Download button"
    ↓
    Automotive SW - Model-Based Design Toolbox
    ↓
    NXP Model-Based Design Toolbox for S32Kxx Automotive Microprocessors Family
    ↓
    NXP_MBDToolbox_S32K1xx_?.?.?_????????.mltbx
    ```
2. [NXP Product Page](https://www.nxp.com/design/design-center/software/automotive-software-and-tools/model-based-design-toolbox-mbdt:MBDT?&code=MC_TOOLBOX)
    ```
    Automotive SW - Model-Based Design Toolbox
    ↓
    NXP Model-Based Design Toolbox for S32Kxx Automotive Microprocessors Family
    ↓
    NXP_MBDToolbox_S32K1xx_?.?.?_????????.mltbx
    ```

#### 2.2.2 Install

Go back to the `NXP Support Package S32K1xx` GUI and click
the `Install MLTBX File as Add-On` button, direct to the installed /
extracted `.mltbx` file.

#### 2.2.3 Verify Install

You should now see `NXP_MBDToolbox_S32K1xx` on your Add-On manager.

![MATLAB NXP MBD Installed.png](pictures/nxp/MATLAB%20NXP%20MBD%20Installed.png)

### 2.3 S32Kxx Model-Based Design (MBT) Toolbox License

#### 2.3.1 Obtain Host ID

You will be promoted for a Host ID later on. The Host ID is either a Volume
Serial Number or a MAC address that restricts your license to 1 computer.

- Note: The license is removable / reversible.

To get your Host ID follow the terminal commands bellow:

##### 2.3.1.1 Windows

Host ID is either the Volume Serial Number of MAC address.

```shell
vol c:
```

```shell
getmac -v
```

##### 2.3.1.2 macOS

Host ID is the MAC address of the en0 device.

```shell
ifconfig en0 | grep ether
```

##### 2.3.1.3 Linux

Host ID is the MAC address any interface. If interfaces are enumerated, use the
lowest-enumerated interface.

```shell
/sbin/ifconfig <interfaceName>
```

#### 2.3.2 Generate License File

Go to NXP Software Portal for Downloads and Licenses.

Use the recommend link options below (in order of highest to least recommended):

1. [NXP's Software Portal](https://www.nxp.com/webapp/swlicensing/sso/downloadSoftware.sp?catid=MCTB-EX)
    ```
    "Download button"
    ↓
    Automotive SW - Model-Based Design Toolbox
    ↓
    NXP Model-Based Design Toolbox for S32Kxx Automotive Microprocessors Family
    ↓
    License Keys
    ↓
    Add with Host ID
    ```
2. [NXP Product Page](https://www.nxp.com/design/design-center/software/automotive-software-and-tools/model-based-design-toolbox-mbdt:MBDT?&code=MC_TOOLBOX)
    ```
    Automotive SW - Model-Based Design Toolbox
    ↓
    NXP Model-Based Design Toolbox for S32Kxx Automotive Microprocessors Family
    ↓
    License Keys
    ↓
    Add with Host ID
    ```

#### 2.3.3 Download License File

The license file might auto download (does not most of the time).

Copy the contents of the license key generated and save it to a file called
`license.dat`.

- If you're on windows create a `license.txt` file with Notepad. Paste the file
  contents, save the file, close, and then rename the file `license.dat`.

The final result should be a `.dat` file with the license key information.

#### 2.3.4 Activate License File

Go back to the `NXP Support Package S32K1xx` GUI and click
the `Activate NXP MBD Toolbox` button, direct to the license `.dat` file.

### 2.4 Verify Complete Setup

Verify the setup on the `NXP Support Package S32K1xx` GUI. All "verify" sub-step
buttons should now be Green.

![MATLAB NXP Completed install guide.png](pictures/nxp/MATLAB%20NXP%20Completed%20install%20guide.png)

- Note: Convert `ZIP to MLTBX` may not be required depending on the download
  process.

---

## 3 Initial Project

1. Open up MATLAB and Simulink.
2. Open up the `NXP Model-Based Design Toolbox for S32K1xx MCUs` tool box tab.
3. Open the `Embedded Coder` app.
   ![MATLAB NXP Toolbox Example.png](pictures/nxp/MATLAB%20NXP%20Toolbox%20Example.png)
4. Build your model, in this example the "GPIO on S32K144 LED & Button" block is
   used.
5. `Generate Code` or `Build` (generate and flash) to generate code and/or flash
   the generated code to a connected MCU.
   ![MATLAB NXP Embedded Coder Build Example.png](pictures/nxp/MATLAB%20NXP%20Embedded%20Coder%20Build%20Example.png)
