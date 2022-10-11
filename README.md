# ESE-5190-Lab-2-Setup-Guide-Sai

``Sai Koushik Samudrala Sambasastry
Tested On: Lenovo Legion 5i, Ubuntu 22.04 LTS
LinkedIn: https://www.linkedin.com/in/sai-koushik-3bbbb8235/``

# Lab #2: Documentation

## 1. Setting up the environment 
### OS Information
I have set up the tools on my laptop running ``Ubuntu 22.04 LTS``

### Software Information

#### Terminal
I will be using Ubuntu's default terminal application (GNOME Terminal) for this project. There is no need to install this as it comes bundled with the operating system.

#### Code Editor
I will be using vim as my primary text editor. I will also use gedit as my secondary text editor, if there is any necessity.

Since vim is already contained in Ubuntu's repositories, its relatively easy to install it by running the following command with root access:

``apt install vim ``

There is no need to install gedit as it comes pre-installed with the OS.

#### Serial Console

##### Screen
Screen is a 'terminal multiplexer', which is included in the repositories of most Linux distributions.

To install this application, I ran the following command with root access:

``apt install screen``

The installation can be verified by running

``screen --version``

##### Determining the serial port of the board
We need to understand the serial port which is being used by the device. The best way to determine this is to run the following command:

``ls /dev/ttyACM*``

**Note: This command must be run after connecting the board.**

If the device discovery is succesful, we get something like this in the output:

``/dev/ttyACM0``

We're now set to access the advanced serial console.

##### Connecting via screen
Accessing the serial console is now as simple as running:

``screen /dev/ttyACM0 115200``

**Note: Please run the above command with root access so that you do not need to worry about setting up a group associated with the hardware and adding a user to that specific group.**

#### Git
Since we will be cloning some git repositories, git must be installed on device, which can be done by running the following command with root access:

``apt install git`` 

##### At this stage, all the required software are installed. We now proceed to set up the SDK in the next section.

## 2. Setting up the SDK
Create a directory named ``pico`` in a suitable location on your computer. We then proceed to clone the repository by running

``git clone -b master https://github.com/raspberrypi/pico-sdk.git``

``cd pico-sdk``

``git submodule update --init``

### Setting up the examples directory
Go back to the folder ``pico`` which was created in the previous step. The following command may be modified accordingly.

``cd ..``

``git clone -b master https://github.com/raspberrypi/pico-examples.git``


### Setting up the toolchain

Most personal computers are based on the x86_64 instruction set, which is sometimes referred to as AMD64. However, the RP2040 runs on an ARM Cortex processor. Since these two instruction sets are not inter-operable, we need to go for cross compilation. This is because the executables which are targeted for x86_64 do not work with ARM processors. This step is used to install the cross compilation tools necessary for our task, since we will be using a personal computer to compile the code which will then be executed on the RP2040. 

We now proceed to install the GNU Embedded Toolchain for ARM by running the following commands as root:


``apt install cmake gcc-arm-none-eabi libnewlib-arm-none-eabi build-essential ``

**All Set!**

## 3. Hello, World!

The examples directory contains numerous illustrations which demonstrate the usage of the SDK. Let's try to compile the *hello world* program and run it over USB.

### Here are the steps:

* Change to the pico directory by modifying the below command appropriately as it depends on the path where the folder was first set up. For me, it is:
``cd ~/Documents/pico``

* Let us check the contents of this directory by running ``ls`` in the terminal.

Output:![Screenshot from 2022-10-10 19-47-07](https://user-images.githubusercontent.com/64246696/194972580-96f714a2-54b6-4214-aad7-35d0fe8b70ff.png)



* A number of examples have been provided in the examples folder. Let's explore this folder. But first, we need to change our directory inside the terminal by running ``cd pico-examples/``

* Again, let's take some time exploring its contents by running ``ls``

Output![Screenshot from 2022-10-10 19-50-09](https://user-images.githubusercontent.com/64246696/194972593-df8dbf5c-63b8-4e24-9ffa-11c146363c8e.png)


* 

* Now, since we are interested in running the hello_world program, let's briefly review the contents of the program by running the following commands:
``cd ./hello_world``

* We are interested in the usb folder since that is the interface which is being used to program the board. So let's go into that directory by running 
``cd ./usb``

* Let's now see the code. Since vim is my preferred editor, let's check it out by running \
``vim ./hello_usb.c``

* Output
![Screenshot from 2022-10-10 19-53-21](https://user-images.githubusercontent.com/64246696/194972605-25696078-8549-4147-9e21-11ae73c6ad91.png)



* I've seen lots of memes on Reddit about how people find it difficult to exit vim. 
![Screenshot from 2022-10-10 19-58-04](https://user-images.githubusercontent.com/64246696/194972618-112c9109-fd62-4f05-99f1-7e27d4608542.png)
![Screenshot from 2022-10-10 19-58-50](https://user-images.githubusercontent.com/64246696/194972631-3ae5914a-c0a0-4981-b415-ec41f01aedf5.png)

So this is the best way to exit vim. Press *escape* and type ``:wq`` to exit the program.

* So let's go ahead and compile the program. To do this, run the following commands:
``cd ~/Documents/pico/``

* Now, let's make a build directory, where the executables will be stored. This is very helpful when using CMake.
``mkdir build && cd build``


* Let's also export the pico-sdk path by running
`` export PICO_SDK_PATH=../../pico-sdk``

* Let's then run ``cmake ..``

* We get the following output:
Output:![Screenshot from 2022-10-10 20-06-52](https://user-images.githubusercontent.com/64246696/194972646-ff283495-e599-47f2-abf7-1196375069c6.png)


* Now, lets `cd` to the ``hello_world`` directory and then into the `usb` folder. Now, let's run the following command:
``make -j4``

* Output
![Screenshot from 2022-10-10 20-09-19](https://user-images.githubusercontent.com/64246696/194972661-b0602acf-b6d9-49f8-af0d-925b2bec58c5.png)


* The executable has now been generated successfully. Let's check its presence by running `ls`

Output
![Screenshot from 2022-10-10 20-10-21](https://user-images.githubusercontent.com/64246696/194972670-d8e63eec-88bd-4b37-9529-e6ea2860604c.png)

* The executable is called hello_usb.uf2

 * We now proceed to connect the board to the computer via USB. Ensure that the boot button is pressed while connecting the device to the computer. If it is detected successfully, it is displayed in the file explorer window

* Output
![Screenshot from 2022-10-10 20-14-19](https://user-images.githubusercontent.com/64246696/194972689-8c4ac698-eb7e-45e3-b3ec-a3fa521f73d3.png)



* We then drag and drop the executable (hello_usb.uf2) to the *RPI-RPI2* device.

* To see the output, we run the screen command by running:

``sudo screen /dev/ttyACM0 115200``

We can now see the output on the terminal window.
![Screenshot from 2022-10-10 20-17-10](https://user-images.githubusercontent.com/64246696/194972705-2899d775-533e-4800-a2e5-3f8752e598fe.png)














