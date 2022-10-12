# C SDK Setup Guide on Mac （M1)

## Computer Information
**MacBook Air(M1, 2020)**

**Chip** Apple M1

**Memory** 8 GB


## Setup Guide 


### Pre-work: MacOS Compatibility Setup
For Macs with Apple Silicon(M1/ M2) is better to use an x86 version of the Homebrew package manager to ease compatibity issues.

* Duplicate the "terminal" in application folder and rename it with "x86" in addtion to differ from the original one:
<img width="343" alt="Picture1" src="https://user-images.githubusercontent.com/114005477/195220550-5aa02a08-0382-4e51-9ebf-2c1fa7facb9a.png">

* Right click, get info and check the box for "open use Rosetta"
<img width="250" alt="Picture2" src="https://user-images.githubusercontent.com/114005477/195221475-8f7c12e2-30e7-4ca4-91ea-fe3b82de6f47.png" >

* Install Homebrew on "Terminal x86"
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```


### Step 1: Installing the Toolchain
After installing Homebrew, then install the toolchain in the terminal:
```
$ brew install cmake
$ brew tap ArmMbed/homebrew-formulae
$ brew install arm-none-eabi-gcc
```
Once the toolchain is installed there are no differences between macOS and Linux.


### Step 2: Get the SDK and Examples
We can download the [pico-sdk](https://github.com/raspberrypi/pico-sdk) and [pico-examples](https://github.com/raspberrypi/pico-examples) directly.

Or use the terminal to create a pico directory at ../user/pico:
```
$ cd ~/
$ mkdir pico
$ cd pic
```
And then clone the pico-sdk and pico-examples git repositories:
```
$ git clone -b master https://github.com/raspberrypi/pico-sdk.git
$ cd pico-sdk
$ git submodule update --init
$ cd ..
$ git clone -b master https://github.com/raspberrypi/pico-examples.git
```
<img width="763" alt="Screen Shot 2022-10-11 at 8 53 19 PM" src="https://user-images.githubusercontent.com/114005477/195224423-33828c36-779e-418f-a537-1776edc64d57.png">



### Step 3: Using Visual Studio Code and Building with CMake Tools
#### a) Installing VSCode

[Visual Studio Code (VSCode)](https://code.visualstudio.com/download) is a cross platform environment and runs on macOS, as well as Linux, and Microsoft Windows. [Download MacOS version](https://code.visualstudio.com/download) and drag it to Applications Folder.


#### b) Building with CMake Tools

* In the left bar find "extension" and search for "CMake Tools", entry the list and click install button.

<img width="600" alt="Screen Shot 2022-10-11 at 9 23 16 PM" src="https://user-images.githubusercontent.com/114005477/195227414-21cfeeed-7727-4a88-87de-aaae6bbef757.png">

* Set the PICO_SDK_PATH environment variable. Navigate to the pico-examples directory and create a .vscode directory and add a file called settings.json to tell CMake Tools to location of the SDK.

```
{
    "cmake.environment": {
        "PICO_SDK_PATH":"../../pico-sdk"
    },
}
```

<img width="600" alt="Screen Shot 2022-10-11 at 9 27 19 PM" src="https://user-images.githubusercontent.com/114005477/195227845-9845ed76-58e5-45e5-8a26-978a902a4a5a.png">

* Then, click on the Cog Wheel at the bottom of the navigation bar on the left-hand side of the interface and select "Settings". Then in the Settings pane click on "Extensions" and the "CMake Tools configuration". Then scroll down to "Cmake: Generator" and enter "Unix Makefiles" into the box.

<img width="600" alt="Screen Shot 2022-10-11 at 3 01 40 PM" src="https://user-images.githubusercontent.com/114005477/195229051-8eadd250-4031-46dc-ac49-700c7302a754.png">

* Finally, go to the File menu and click on "Add Folder to Workspace..." and navigate to pico-examples repo and click "Okay". Select "GCC for arm-none- eabi" for your compiler. After download it, click on the "Build" button in the blue bottom bar. And this generate a new build folder with elf, bin and uf2 targets.

<img width="250" alt="Screen Shot 2022-10-11 at 9 46 23 PM" src="https://user-images.githubusercontent.com/114005477/195229881-e18635a4-40d1-4bab-b81a-ec9dfd64d376.png">



### Step 4: "Hello World" Example
#### a) Serial Console on Mac
Open the terminal and type following to obtain the board_name:

```
ls /dev/tty.*
```

And then type the following code to connect with screen (remember to replace the board_name with the one obtain above):

```
screen /dev/tty.board_name 115200
```

#### b) Connect to RP2040

Connect the RP2040 with a micro-usb cable. Hold down `BOOTSEL` button and at the same time hold `RESET` button, and then release `RESET` button. We can find the Mass Storage Device on the screen. Drag the `uf2` file to it, it will eject automatically. Then on the terminal we can find it prints "Hello,  world!"

<img width="646" alt="Screen Shot 2022-10-11 at 5 59 53 PM" src="https://user-images.githubusercontent.com/114005477/195354355-27c3ba78-5e7b-4bcc-b1fd-f7abcd8ec617.png">

