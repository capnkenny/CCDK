CCDK setup instructions
====

The steps required for setup are as follows:

1. Download the CCDK repository using the git tool.
2. Install any required external tools such as Ruby and Python.
3. Check the operation of the CCDK sample program.
4. Operation check on Linux and MacOS X


In steps 1 to 3 above, we will first explain the parts that can be completed using Windows alone.
Things related to Linux and MacOS X will be explained in the final section, 4.



###1. Download the CCDK repository using the git tool.

The location of the git repository is:

~~~
https://github.com/capnkenny/CCDK
~~~

You can get the entire CCDK by cloning this repository.
When you clone it, you will get information including all versions.
You can always get the version you need later using the git checkout command.

If you are familiar with how to use git, you can skip the following explanation and proceed to step 3, Installing external tools.

The git program is available on various operating systems such as Windows, Linux, and MacOS X.
There are a variety of options available, including command line and GUI.

Since CCDK is mainly a tool for Windows, the following will use GitHub for Windows provided by GitHub, Inc.
Let me explain using an example.

First, download and install GitHub for Windows from the following URL.

~~~
https://windows.github.com/
~~~

Once the installation is successful, you can launch the program by going to "All Programs" → "GitHub, Inc" → "GitHub".

Launch the GitHub program and click the "+" button in the top left of the screen to add a repository.

Add means to add an existing directory, Create means to create a new one, and Clone means to download a remote repository on GitHub.
Here, click Clone.

If you are part of the ShinraTech team, you will see the ShinraTech icon, as shown below.

![githubclone](images/github_clone_ccdk.png)

Clicking the icon will display a list of ShinraTech repositories such as CCDK, hiredis, and redis, so select CCDK. This will execute the clone, and the download should be completed within a few minutes.

The downloaded repository is, by default, located under the username cloud.
It will be placed in the following location. This default location is GitHub for Windows
This frequently changes depending on the version, so please open the settings from the gear icon to check.

~~~
C:\Users\cloud\Documents\GitHub\CCDK
~~~


###2. Install required external tools such as Ruby and Python.
CCDK uses the following external tools.
The following explains only the Windows version. Linux (UNIX) will be explained later.

* Python 3.4 (3.4.1)
* Ruby 2.1 (2.1.5p273)
*VisualStudio 2013
* Redis (MSOpenTech version)
    

Although it should work even if the last number (minor version) of the version number does not match, the development team has only tested these versions.
If it doesn't work or you need to verify it with another version,
Please speak to a member of our development team.

For Python 3.4, please use the following URL.

~~~
https://www.python.org/downloads/windows/
~~~

python.exe is usually installed in the following path. Please add this to the environment variable PATH.

~~~
C:\python34\python.exe
~~~

Start cmd.exe or similar, type python, and if the following is displayed, the command was successful.
![pythoncmd](images/cmd_python.png)

You can download the installer for Ruby 2.1 from the link below.

~~~
http://rubyinstaller.org/downloads/
~~~

Ruby.exe is usually installed in the following path. Please add this to the environment variable PATH.

~~~
C:\ruby21-x64\bin\ruby.exe
~~~

For ruby, just type ruby ​​on the command line like you would for Python.
If the prompt appears, the installation was successful.


For VisualStudio 2013, the free version is acceptable.
For CCDK development, we use Professional 2013.

The free version can be downloaded from the Microsoft website below.

~~~
https://www.visualstudio.com/downloads/
~~~

Redis is a fork of the official Redis project.
When compiling Microsoft's open source version with Visual Studio,
A library file for Redis will also be generated, so we will use that.

If you clone the Redis source code using GitHub for Windows,

~~~
CCDK/externals/redis
~~~

It has been checked out to .

~~~
CCDK/externals/redis/msvs/RedisServer.sln
~~~

Open the solution file in this location and build the whole thing.

~~~
CCDK/externals/redis/msvs/Debug/hiredis.lib
CCDK/externals/redis/msvs/Debug/redis-server.exe
~~~

etc. will be output.
We will use these later when building the backend server.
Build both Debug and Release.


###3. Check the operation of the CCDK sample program.

The following programs can be checked for operation on a single Windows machine.

1. 1:1 skeleton program (Windows)
2. N:N skeleton program (Windows)
3. Backend Server Programs

Checking the operation of the entire CCDK can be broadly divided into two stages.
First, we performed a test without video streaming.
Once that works, we'll test the same program using video streaming.

In addition, the back-end server will be checked using the N:N skeleton program.
There are versions for Windows and UNIX (Linux or OS X).
Testing using the Linux version will be described later.

So, the overall process would be as follows:

1. Build the whole thing.
2. Check the 1:1 skeleton without video streaming
3. Checking N:N skeleton without video streaming
4. Check the 1:1 skeleton with video streaming
5. Checking N:N skeleton with video streaming


<B>Step 1, Build the whole thing first.</B>

At the top level of the CCDK repository, there is a file called CCDK.sln, so open it (Figure).

![ccdksln](images/ccdk_sln.png)

Simply run "build" and the whole thing will be compiled.
In this case, a package called DirectX ToolKit will be automatically restored (installed) by NuGet.

<B>Step 2, Check the 1:1 skeleton without video streaming.</B>

To try it out, set one_to_one as the startup project and start a debug run.

![one_to_one_run](images/one_to_one_run.png)

If you see a window with a continually increasing frame counter as shown above, then you've succeeded.
Close the window or press Q to quit.

<B>Step 3: Check the N:N skeleton without video streaming.</B>

Let's try starting many_to_many without starting the backend servers.

![many_to_many_no_backend](images/many_to_many_run_without_backend.png)

Compared to one_to_one, there are more "Ping:0" and "Channelcast:0".
Ping:0 means that the number of pings returned to the backend server is 0.
This means that you are not connected. The same is true for Channelcast:0.

Close the program and then start the backend server.
As our goal is not to debug right now, we will start the backend project without the debugger.
If you enable the database function of the backend server, you will also need to start the Redis server, but the default setting in CCDK.sln does not allow the database function to be used.
We will discuss using Redis later.

When the backend server starts, the console will be displayed and logs will be output as shown below.
This log is also copied to the Visual Studio debug output.
![realtime_backend_run](images/realtime_backend_run.png)

If you want to run it with the debugger, please increase the number of instances of Visual Studio.

Furthermore, if you launch the many_to_many project without the debugger, you will see the following image.
![many_to_many_run_one](images/many_to_many_run_one.png)

The Ping counter should increase, indicating successful communication with the backend server. However, Channelcast remains at 0, because Channelcast does not send to itself.

Let's start another many_to_many.

![many_to_many_run_two](images/many_to_many_run_two.png)

Now you will see that the Channelcast value increases by 1 every frame.
The current many_to_many program is for the channel (ID=12345):
One channelcast packet is sent every frame.
The more programs you have connected to the backend,
You will receive a number of channelcast packets proportional to the square of that number.
Run some more many_to_many calls and see how fast packets are received.



<B>Step 4, Check the 1:1 skeleton with video streaming</B>

To try video streaming, use the
You need the MCS package. You can extract the MCS package anywhere.
Here, it will be deployed in the same location as it is placed in the CCDK (Figure).
![ccdk_mcs_content](images/ccdk_mcs_content.png)

The number 9101.8 in the figure is the MCS build number, which changes every time a new version is released.

Once the extraction is complete, run ShinraDevelopmentStation.exe.
If you are running an unsigned program, a Windows warning will appear, but please select Run. From now on, this program will be referred to as "SDS".

SDS uses Python internally, so you will need to set up Python.

When you run it, you will see an empty window like this:
![sds_startup](images/sds_empty.png)

SDS is a GUI tool for creating and using game program packages, and is described in detail in a separate document.

First, check your SDS settings.
Select ``Settings > MCS Configuration`` from the menu and the settings dialog will appear as shown below.
![](images/sds_mcs_configuration.png)

The top specifies the location of the Python executable. If the file does not exist, it will be displayed with a red frame.
The Shinra script path and Shinra MCS path will be automatically filled in with the location of the launched SDS.
The Games installation dir is where SDS stores various files during operation, including copies of all files that make up the game. If the directory does not exist, create it. You can leave the Default game user id as is.


Next, create a project.
Select ```File > New``` from the menu.
A window called New Shinra Project will appear.
Enter the name of the file (something like .vcxproj) to save your project settings.
Let's call it sdstest.
You can save the file in any location, such as the Documents folder.
Next, select ```Project > Add Data pack``` from the menu.
Then the following window will appear:
![](images/sds_add_datapack.png)
A DataPack is a directory that contains all the files necessary to run the game.

To allow you to quickly try out SDS, there is a directory called ``CCDK/streamtest``` at the top level of the CCDK repository. This directory contains one_to_one.exe, many_to_many.exe, and the necessary image files (Figure 1).
![](images/streamtest_dir.png)
Just to be on the safe side, double-click one_to_one.exe or similar to launch it and check that it works.

Add this directory as a DataPack as is.
From the menu, select Project > Add Data Pack.
Click the Browse button and select the streamtest directory. If successful, the tree view on the left will display ``` sdstest > Data packs > DataPack ``` as shown below.
You can add multiple DataPacks, but for now we'll just add one.

![](images/sds_add_streamtest.png)

Next, in the Startup section, you specify which file in this DataPack is the executable to be launched.
Select Project > Add Startup configuration and you will see the following screen:
![](images/sds_add_startup.png)

The Data pack entry has a red border, this indicates that a DataPack has not yet been selected, click on the red border to select a DataPack that has already been added.

Next, enter one_to_one.exe directly into the Executable input field. When the red frame disappears, input is complete.

After the Executable, specify the Working directory. Since we want to run in the top directory of the DataPack, specify "." (without the double quotation marks).


![](images/sds_set_executable_workdir.png)

Finally, check the operation. Select ```Project > Start game > Startup ```.

![](images/sds_start_game_menu.png)

If you select Startup, the Start button as shown below will be displayed.

![](images/sds_running_game.png)

Some information such as the username and port number to be used is displayed, but we will explain that in another document. For now, just press the Start button.

Two windows will be displayed (the first time you run it, a Windows warning will appear, but always select Allow). One is "Direct3D Win32 Game1", which is the window for one_to_one.exe. The other is ShinraClient, which is the video stream viewer.

![](images/sds_start_game_and_client.png)

When you focus on ShinraClient and press Enter, you will see the video stream as shown below.
You can see that the image quality is different from that of a video stream. You can also see that the timing is slightly delayed when you press the P key to play the sound. Please note that the quality and delay of the video and audio are different from those of Shinra System's commercial service.
The one_to_one.exe window will remain blank since it is not drawing to the screen.

![](images/sds_one_to_one_stream_work.png)

Close ShinraClient and press the Stop button on the SDS to end the test.


<B>Step 5, Check the N:N skeleton with video streaming</B>

The directory CCDK/streamtest specified as DataPack in step 4 contains the following:
Many_to_many.exe is also included. Just add another Startup using SDS.
You can try many_to_many video streams.

First, start the backend server from CCDK.sln using the method in step 3.
This can be done with or without a debugger.

Next, select Project > Add Startup configuration in SDS, add DataPack as Data pack, many_to_many.exe as Executable, and specify "." as the Work directory as in step 4 (Figure).
![](images/sds_add_many_to_many_startup.png)

At this point, you can see in the tree view on the left that a new Startup configuration called Startup1 has been added.

Once added, select ```Project > Start game > Startup1``` and start it as in step 4. If the video stream is displayed and the Ping value increases, it is a success (Figure).

![](images/sds_many_to_many_stream_work.png)

To launch multiple many_to_many.exe instances, right-click on the grey area of ​​the window where the Start button is displayed to display the popup menu, and select ``Add game instance``` (see image).
![](images/sds_add_game_instance.png)

You will then be able to launch two games with different port numbers assigned, as shown in the image below.

![](images/sds_two_instances.png)

Press the respective Start buttons to launch the two sets of many_to_many.exe and ShinraClient (Figure).

![](images/sds_two_many_to_many_stream_work.png)

When you see the Channelcast counter value increasing, it's done.
You can add any number of clients in the same way.



###4. Operation check on Linux and MacOS X
Only the backend server can run on Linux or MacOS X.
The steps are as follows:

1. Setting up Linux itself
2. Install required external tools and libraries
3. Get CCDK with git
4. Build the backend server
5. Start the Redis Server
6. Start the backend server


Step 1: Set up Linux itself

CCDK has been tested on Ubuntu server 14.04.2 or 10.04.
In particular, it does not use new kernel features,
We provide everything you need in source code,
It should be fine for most distributions.

If you are familiar with setting up Linux, you can skip to step 2.
Also, if you are using MacOS X instead of Linux, this step is not necessary.

Here, we will use VirtualBox and Ubuntu Server.
We will show you how to build a local Linux server environment completely free of charge.

First, download VirtualBox from the official website.

[http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html](http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html)

The host OS can be either MacOS X or Windows.

Next, download Ubuntu Server from its official website.
[http://www.ubuntu.com/download/server](http://www.ubuntu.com/download/server)

The file you need will be something like ubuntu-14.04.2-server-amd64.iso , it will be around 600MB in size. It doesn't matter if the version number is slightly different. Ubuntu-desktop is fine too.

Start VirtualBox, click the New button, select Linux, Ubuntu 64bit,
The memory capacity is set to about 2GB, and the disk capacity is variable and set to about 20GB.
When you start the program, you will be prompted to enter the file location of the disk image.
Select the ISO file you just downloaded.
After that, just like a normal Ubuntu installation, set the keyboard, time,
Please set the user account name, etc.
![](images/ubuntu_1404_setup_done.png)
When you see the login console like the one above, the setup is complete.




<B>Step 2, Install the required external tools and libraries</B>

Below, we will explain how to do it in Ubuntu.
In the case of Ubuntu server, the initial configuration is minimal, so
git, make, gcc, ruby, ruby-dev, racc, g++, zlib1g-dev, redis-server
You need to install them. Install them as follows:

~~~
bash$ sudo apt-get install git
bash$ sudo apt-get install make
bash$ sudo apt-get install gcc
bash$ sudo apt-get install ruby
bash$ sudo apt-get install ruby-dev
bash$ gem install racc
bash$ sudo apt-get install g++
bash$ sudo apt-get install zlib1g-dev
bash$ sudo apt-get install redis-server
~~~

Next, install hiredis.
hiredis is a library for programmatic access to Redis servers.
On Ubuntu, hiredis is not provided as a package.
So you need to build it from source.

The source code for hiredis can be obtained and built using the following command line.

~~~
bash$ git clone git@github.com :ShinraTech/hiredis
bash$ cd hiredis
bash$ make
bash$ make install
~~~

This is all for Ubuntu 10.04. I haven't tested it on other distributions, but
You should be able to install it in a similar way.
On Linux, the hiredis shared library is required when starting the backend server, so
It is a good idea to write the following environment variables for the dynamic linker in .bashrc or similar.
To find the actual location of your shared libraries, look at the output from make install.

~~~
LD_LIBRARY_PATH=/usr/local/lib
~~~



For MacOS X, with XCode and command line tools installed,
In a Homebrew environment, the only additional required dependencies are redis, hiredis, and racc.

~~~
bash$ brew install redis
bash$ brew install hiredis
bash$ gem install racc
~~~

That's all.

<B>Step 3, Get CCDK with git</B>
This applies to both Linux and MacOS X.
On MacOS X, you can use the GUI app [GitHub for Mac](https://mac.github.com/),
Here we explain the command line method.

~~~
bash$ git clone git@github.com :ShinraTech/CCDK
~~~

That's all you need to do.
At this stage, submodules such as redis and moyai have not yet been fetched.


<B>Step 4, Build the backend server</B>

This applies to both Linux and MacOS X.

A Makefile for building the entire project at once is provided in the CCDK top directory.

~~~
bash$ cd CCDK
bash$ make setup
~~~

By running make setup, all git submodules will be obtained and compiled.
Once compilation is complete, two programs, ssv and ssbench, will be created in the backend directory (Figure). ssv is the backend server. datadir is the directory that stores static files and is required for ssv to work.

![](images/backend_compiled.png)

In the backend directory, start ssv as follows:

~~~
bash$ ./ssv
~~~

If hiredis cannot be found because LD_LIBRARY_PATH is not set, the following error may appear and it may not start.

~~~
./ssv: error while loading shared libraries: libhiredis.so.0.12: cannot open shared object file: No such file or directory
~~~

If you start ssv without any arguments, it will display a description of how to use it and then exit (Figure).
![](images/ssv_noopt_run.png)


Step 5: Start the Redis server
SSV uses Redis rather than a SQL server to persist data.
To start Redis, run

~~~
bash$ redis-server
~~~

When you start it, the ASCII art logo will be displayed (see image).
![](images/redis_server_run.png)


<B>Step 6, Start the backend server</B>
Go to the ```CCDK/backend``` directory and

~~~
bash$ ./ssv realtime database
~~~

When started as above, SSV will start operating as shown in the figure below.
![](images/ssv_opt_run.png)




