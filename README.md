# Yocto on WSL2

I wanted to try running on WSL instead of a real linux.



## Introduction
This document assumes WSL2 is already installed. 
I used WSL2 on Windows 10 2004.
The Yocto part of this experiment is based on:

https://www.blaess.fr/christophe/yocto-lab/sequence-I-2/index.html


## Installing the WSL linux distribution on a non-boot drive
The first hurdle I had to overcome was to not install the linux distribution the standard way because when using the Microsoft Store to install Ubuntu in WSL it is installed on the boot drive under the current user AppData folder.

Knowing the size Yocto can use to create image, I knew this was not practical for me.

So here are the steps:
- Create a folder in a non-system drive (In my case on F: my brand new 1TB SSD)
- Run the following commands in PowerShell to create a folder for the installation:

  `cd F:\ `
  
  `mkdir WSL `
  
  `cd WSL `

- Download a Linux distro: 

  `Invoke-WebRequest -Uri https://aka.ms/wsl-ubuntu-1804 -OutFile Ubuntu.appx -UseBasicParsing`

- Unpack the downloaded distro and initialize linux:

  `move .\Ubuntu.appx .\Ubuntu.zip`
  
  `Expand-Archive .\Ubuntu.zip`
  
  `cd .\Ubuntu\`
  
- Looking in the folder with the Windows Explorer one can see a .vhdx file that is the linux file system.

- Run the executable to launch linux:
  `.\ubuntu1804.exe`

- Enter a user name and password and you are ready to go.


## Yocto
From here on we can start to follow the excellent Yocto tutorial from Christophe Blaess.

...

After installing all the Yocto base image the .vhdx file that represent the linux file system has tremendously grown (over 40GB). This was the reason I could not do this on my boot drive.

![alt text](https://github.com/phicore/Yocto-on-WSL2/blob/main/vhdx-size.png "vhdx file size")

## Testing the image

When launching the command to test the image (runqemu qemux86-64)

It is clear that we miss a X11 windows server as the qemu runs under SDL+X11

## Installing a X11 server 

In Windows(not WSL), download and install MobaXterm (free version): https://mobaxterm.mobatek.net/download.html

Download the portable version and add it for instance in the WSL folder.

Launch MobaXterm_Personal_X.x.exe, it shall show the installed distributions, select the one you need and a terminal is launched.
![alt text](https://github.com/phicore/Yocto-on-WSL2/blob/main/MobaXTerm.png "X11 server")

This time when launching a sample X11 app like xeyes, the application runs


In the terminal: 
repeat the source ... command from the Ycoto tutorial.

And then:

 `runqemu qemux86-64`

![alt text](https://github.com/phicore/Yocto-on-WSL2/blob/main/quemu-success.png "qemu-sdl running and poky has booted")

Now the qemu windows is correctly running and the image build with Yocto is booting inside qemu

From here on Windows 10 can be used to host a ycoto developement (at least for the image generation steps)

