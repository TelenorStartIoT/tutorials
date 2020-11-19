# Compile and Build for Thingy:91 and nRF9160 DK

This tutorial will guide you how to setup your environment for programming and building your own firmware for the Thingy:91 or nRF9160 DK.

## 1. Prerequisites

### 1.1 GNU Arm Embedded Toolchain

The [GNU Arm Embedded Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) contains necessary software such as the compiler itself. You must download the toolchain and setup for your operating system. You can find more information about this step in the [nRF Connect SDK documentation](http://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/gs_installing.html#installing-the-toolchain).

#### Windows

1. Download the latest release for Windows: https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads

2. Extract the toolchain into a folder of your choice. We recommend to use the folder `C:\gnuarmemb`.

3. Define environment variables for the GNU Arm Embedded toolchain. Press "Start" and type "environment variables" to open "Edit the system environment variables" section in the Control Panel:

![](https://github.com/TelenorStartIoT/tutorials/blob/master/07-thingy-program/assets/00-edit-env.PNG)

Press the "Environment Variables..." button at the bottom:

![](https://github.com/TelenorStartIoT/tutorials/blob/master/07-thingy-program/assets/00-edit-env-2.PNG)

Create two new environment variables by pressing "New". The name anv value for each new environment variable should be the following:

| Name                     | Value        |
| ------------------------ | ------------ |
| ZEPHYR_TOOLCHAIN_VARIANT | gnuarmemb    |
| GNUARMEMB_TOOLCHAIN_PATH | C:\gnuarmemb |

It should look like this:

![](https://github.com/TelenorStartIoT/tutorials/blob/master/07-thingy-program/assets/00-edit-env-3.PNG)

Click "OK" and "OK".

#### Linux and macOS

1. Download the latest release for Linux or macOS: https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads

2. Extract the toolchain into a folder of your choice. We recommend to use the folder `~/gnuarmemb`.

3. Add the following content to the `~/.profile` file:

```
export ZEPHYR_TOOLCHAIN_VARIANT=gnuarmemb
export GNUARMEMB_TOOLCHAIN_PATH="~/gnuarmemb"
```

### 1.2 West

West will help us get a coherent version of the NCS (nRF Connect SDK) that we need in order to build firmware. You can find more information about this step in the [nRF Connect SDK documentation](http://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/gs_installing.html#installing-west).

To install west, simply run the following command (requires pip3):

```
pip3 install west
```

### 1.3 CMake

Instructions for how to install CMake for Windows will be described here. If you're using Linux or macOS you should be able to install it from your respective package repository.

1. Download the latest stable release of CMake [from here](https://cmake.org/download/).

![](https://github.com/TelenorStartIoT/tutorials/blob/master/07-thingy-program/assets/00-cmake.PNG)

![](https://github.com/TelenorStartIoT/tutorials/blob/master/07-thingy-program/assets/00-cmake-1.PNG)

2. Start and finnish the installation. When asked, make sure to select **Add CMake tot the system PATH for all users**:

![](https://github.com/TelenorStartIoT/tutorials/blob/master/07-thingy-program/assets/00-cmake-2.PNG)

### 1.4 Ninja

#### Windows

1. Download the latest binary for Windows: https://github.com/ninja-build/ninja/releases

2. Extract the contents into a folder of your choice. We recommend to use the folder `C:\ninja`.

3. Add the folder to the PATH environment variable. Press "Start" and type "environment variables" to open "Edit the system environment variables" section in the Control Panel:

![](https://github.com/TelenorStartIoT/tutorials/blob/master/07-thingy-program/assets/00-edit-env.PNG)

Press the "Environment Variables..." button at the bottom:

![](https://github.com/TelenorStartIoT/tutorials/blob/master/07-thingy-program/assets/00-edit-env-2.PNG)

Locate and select the "Path" environment variable, and click "Edit":

![](https://github.com/TelenorStartIoT/tutorials/blob/master/07-thingy-program/assets/00-env-path.PNG)

Add a new entry with the value `C:\ninja`:

![](https://github.com/TelenorStartIoT/tutorials/blob/master/07-thingy-program/assets/00-env-path-2.PNG)

## 2. Get the Code

We should now have every prerequisit in order to start compiling.

Create a new folder on your computer and run the following command inside of it:

```
west init -m https://github.com/TelenorStartIoT/thingy91-nrf9160-dev-kit-coap
```

This will fetch the correct NCS version based on the `west.yml` file in this repository. Once the command has finished run the following command:

```
west update
```

You may also need to install required Python dependencies:

```
pip3 install -r zephyr/scripts/requirements.txt
pip3 install -r nrf/scripts/requirements.txt
pip3 install -r bootloader/mcuboot/scripts/requirements.txt
```

## 3. Compile

Run the following command from the root of your newly created folder:

```
west build telenor-coap/ -b thingy91_nrf9160ns --pristine
```

Change `thingy91_nrf9160ns` to `nrf9160dk_nrf9160ns` if you want to build for the nRF9160 DK instead of the Thingy:91.

## 4. Flash

You will find the final product under `build/zephyr/`. Depending on which device you want to flash you must choose the correct .hex-file:

- Thingy:91 with MCUboot: `app_signed.hex`
- nRF9160 DK: `merged.hex`

### 4.1 Flash Thingy:91

Use the nRF Connect Programmer desktop application to flash Thingy:91 in MCUboot mode with the `app_signed.hex` file. You can follow the [/tutorials/thingy91-flash-with-new-code-hex/](Flash your Thingy:91 with New Firmware (pre-compiled .hex file)) tutorial for instructions.

### 4.2 Flash nRF9160 DK

Run the following command while the nRF9160 DK is connected via a USB cable:

```
west flash
```
