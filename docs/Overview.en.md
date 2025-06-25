# Overview of CCDK
CCDK stands for "Community Cloud Development Kit" and is a software kit for developing cloud games provided by Shinra Technology.

This repository (https://github.com/ShinraTech/CCDK) distributes CCDK documentation, sample games with source code, and binary archives of development tools.


Purpose of CCDK
====
Shinra Technology's purpose in providing cloud gaming technology through CCDK is to:
The goal is to make it possible to create commercial games without having to work long hours full-time at a game company.

I don't have a huge budget. I can't work full time on my game.
There is no dedicated development machine or expensive development tools.
They don't have a dedicated testing team or IT support team.
CCDK is an attempt to make it possible to create commercial games even in such situations.

Wishlist
====
With just one person or a team of 2-3 people,
How much time does it save the programmer?
The key is to develop the game with as little effort as possible.

CCDK is using cloud gaming technology to solve the problems that have been plaguing game development up until now.
We will reduce tedious tasks and expenses unrelated to the game content as much as possible.

In the current version, not all of the following can be achieved,
We continue to develop to make all of this a reality.

1. "It can be made cheaply."

- You can develop by adding free software to your personal PC, such as VisualStudio Express, DirecX, Python, VirtualBox, Linux, etc.
- Project review is required, but you can use Shinra's actual data center equipment free of charge.
- You can provide your game to any platform by just creating a Windows binary (currently only Windows).
 
2. "Quick release"

- Technically, any game engine that supports DirectX can be used
- Implementing multiplayer games becomes extremely easy
- Developing netcode is easier since communication is only within the LAN.
- Very easy to implement billing

<B>3: "I can release with confidence"</B>

- Public testing can be done without distributing programs and data.
- No need to worry about distributing malware to users.
- Since copies are not distributed, version control is ensured and the service can be stopped.

<B>4: "It's easy to get feedback."</B>

- Just send an email with a shinra:// link. Once the recipient clicks on the link, they can start playing right away.
- As long as you save the Shinra pack, you can always provide a working version of the past.
- By outputting the debug log from the game, you can quickly extract gameplay video from the corresponding time.
- Automatic recording of operation and gameplay videos



Technical Support
====
Shinra Technology's development team provides technical support for CCDK.

If you have any questions or concerns, please email us at ``` CCDK-jp@shinra.com ``` in Japanese or ``` CCDK-en@shinra.com ``` in English .

In addition to email, we also have a <a href="https://www.facebook.com/pages/Shinra-Community-CDK/1613401802228319">CCDK Facebook group</a>.
In addition to writing about problems you are having trouble with, you can also write about casual conversation or things you would like to do, so please feel free to write about these things.

We have also prepared a [FAQ](FAQ.ja.md), so please refer to it when you have any questions.

What you can and can't do with CCDK
====
With CCDK, you can:

1. Game creation and test play
With CCDK, you can develop games using three communication models, 1:1, 1:N, and N:N, and send the local rendering results to a remote video stream to actually experience the image quality and operation delays when using the Shinra system's production environment, and check the operation of communications using a back-end server. (As of April 2015, 1:N development is not yet possible.)
2. Create a package file to send to the actual Shinra system environment, and test play it locally to make sure it is not corrupted.


The following cannot be done with CCDK:
1. Publishing (distributing, selling, etc.) games for purposes other than development, particularly publishing games to an unspecified number of customers, including end users of the Shinra Platform.
2. Tune the parameters of the Shinra System's remote renderer
3. Send it to the actual Shinra system environment, test play it remotely, and see what the end user will actually experience.
*This function is currently under development as of April 2015.

MCS (Minimal Cloud Set)
====
MCS stands for Minimal Cloud Set.

MCS is a minimal emulator of Shinra Systems' remote renderer.
CCDK is designed based on MCS.

The diagram below compares the Shinra system's core system (the system for commercial services), MCS, and a game without the Shinra system.

<img src="images/without_shinra.png" width=150>


First, the diagram above shows the state of running a game server using DirectX without the Shinra system.
This is the state that general games that use DirectX are in.
The GPU and display are also installed on the same machine as the CPU.
D3D9.dll is not faked here, we just use the one provided by Microsoft.

![CoreSystem](images/coresystem.png)

The diagram above shows the configuration of the Shinra System's commercial version system.
The CPU and GPU machines are physically separated and connected within the data center LAN using an ultra-high-speed network of over 10 Gbps.
The Game Server runs on the CPU machine, and the Renderer runs on the GPU machine.
The end user (the person playing the game) uses a Client program to send game control information to the game server via a TCP socket.
D3D9.dll is faked, so when you call a drawing function, the call is sent directly from the game server to the rendering server over a high-speed network.
When the rendering server receives a drawing command, it uses the GPU to draw and encode it, then sends the result back to the client over a TCP stream.
This is the "remote rendering function" that is a feature of the Shinra platform.

<img src="images/mcs.png" width=400px>

The diagram above shows the configuration of MCS. Unlike commercial systems, the CPU machine and the GPU machine are not physically separated, and rendering commands are not sent over the network. However, D3D9.dll is faked, and rendering and video compression are performed directly using the GPU on the same machine. The client sends operation information and receives video signals over the network (TCP) in the same way as commercial services.

In MCS, there is a network between the client and the game server, so you can adjust the network settings to simulate a slow network, verify the feeling of delays in game operations and screen rendering, and actually check the image quality and sound quality. (In terms of image quality, only the default settings can be seen in MCS. The image quality will change if you tune the parameters of the core system.)

The MCS included in CCDK is designed to allow game development for the Shinra system to be performed in an environment as close as possible to the commercial version at Shinra Technology's data center, and verification work to be performed, but it is not exactly the same as the functions supported by the commercial version DLL. Therefore, when a game developed with CCDK is run on a commercial version system, unknown problems may occur. In such cases, Shinra Technology will provide technical support.


The latest version of MCS is stored in the packages directory.
The file name is as follows:
There are multiple versions stored, but you should usually use the most recent one, which has the highest number.

~~~
packages/ShinraMCS-8304.34.zip
~~~


Actual game development at CCDK
====
When developing games with the CCDK, you will spend most of your time working in standard Windows mode, without having to fake the Direct3D DLLs.
Then, when necessary, for example just before release, MCS is used to check compatibility and performance with the fake DLL, and once that is confirmed, it is actually deployed to a machine in Shinra Technology's data center.

With CCDK, you don't need to build your game from scratch.
Even if a game is already in development or has been completed, if the game program uses Direct3D, it can be made compatible with the fake DLL of CCDK without any problems. However, since the fake DLL does not fully support all the usage methods of all the functions of Direct3D, verification using MCS is still required.


CCDK test server
====

By using the SDS (Shinra Development Station) included in MCS to package all files involved in running the game (including the back-end server) and send them to a test server provided by Shinra Technology, you can test the game in an environment that is almost identical to the commercial servers owned by Shinra Technology.

The steps to use the test server are as follows:

1. Implement and verify game programs and data without using MCS
2. Verify streaming play using MCS (fake DLL)
3. Use SDS to create a ShinraPack (a zip file in a special format)
4. Send ShinraPack to the CCDK test server (As of April 2015, the following is in preparation)
5. Reserve an environment on the CCDK test server
5. Test your game on the CCDK test server

Once the game has been verified on CCDK's test server, the next step will be to enter into the necessary publishing agreement with Shinra Technology, deploy it to Shinra Technology's commercial service server, conduct further verification, and prepare the service for end users.
Regarding servers for Shinra Technology's commercial services, detailed adjustments are required depending on the game content and country, so please contact CCDK's technical support staff individually.


Obtaining CCDK
====
CCDK is distributed as a git repository on GitHub, not as a compressed file.

The command line to clone is

~~~
git clone https://github.com/ShinraTech/CCDK
~~~

In addition to cloning, you can also freely fork and use it.

When using this in a development project, it is useful to pin versions using git tags corresponding to each release.


Setting up CCDK
===
For setup instructions after cloning, please refer to the "CCDK Setup Documentation".

*The company names and product names listed are trademarks or registered trademarks of the respective companies.