Frequently asked questions and answers about CCDK
====
- <B>What is CCDK?</B><BR>
CCDK is an abbreviation for "Community Cloud Development Kit" and is a development toolkit for small development companies and individuals to create games for the cloud.
<BR><BR>
- <B>What do I need to start developing?</B><BR>
To create a game using CCDK, you will need a Windows 7 machine, DirectX 11, and a development environment of your choice (VisualStudio 2012 or 2013 have been tested). To develop an N:N model game that requires a back-end server, you will also need a server environment such as Linux, if necessary.
<BR><BR>
- <B>Where can I get it?</B><BR>
CCDK is distributed, updated, and supported by GitHub.
<BR><BR>
- <B>When will it be released?</B><BR>
The initial version will be released on Friday, May 15, 2015. This version will be in Japanese. The English version will be released later this year.
<BR><BR>
- <B>Do I need knowledge of network programming to create a game with CCDK?</B><BR>
This is not necessary for the 1:1 and 1:N models. The N:N model requires knowledge of implementing multiplayer games on a LAN, but this can be achieved more easily using a standard data synchronization server or VCE libraries.
<BR><BR>
- <B>What can't CCDK do?</B><BR>
It may be difficult to maximize the GPU performance of the Shinra system. For example, special parameter tuning of the remote renderer is not possible. Please contact technical support for details.
<BR><BR>
- <B>Do I need to have a fiber optic connection to develop a game using CCDK?</B><BR>
No, you can develop your game entirely on your local machine and local network.
<BR><BR>
- <B>Do I have to share my game with Shinra Technology?</B><BR>
It is possible not to share information. However, we believe that moving in the direction of sharing information will enable us to highlight the uniqueness of the Shinra System.
<BR><BR>
- <B>Are there any setup fees to become a Shinra Technology developer?</B><BR>
There is no cost. However, developers will need to prepare their own game development environment.
<BR><BR>
- <B>Does the Shinra platform offer an early publishing system?</B><BR>
We are currently exploring the possibility of releasing premium content earlier.
<BR><BR>
- <B>Do Shinra Technology games offer virtual reality or augmented reality experiences?</B><BR>
Shinra Technology's remote rendering technology can also be useful in a number of emerging technologies, and we would love to discuss this with potential partners in those areas.
<BR><BR>
- <B>I'm a game developer and I have a game concept, who should I contact?</B><BR>
Shinra Technology is neither a developer nor a publisher, but we encourage self-publishing on our platform. AAA developers are
For inquiries, please contact DevJ@shinra.com . If you are an indie developer, please contact CCDK-JP@shinra.com .

<BR><BR>
- <B>I'm a student and I'm interested in systems. How do I get started?</B><BR>
The best place to start is with Shinra Technology's CCDK. Check out our tech blog for more details: https://tech.shinra.com/jp
<BR><BR>
- <B>Do all games on the Shinra platform run on CCDK?</B><BR>
No. CCDK is designed for small businesses and individuals and has lower hardware requirements than development environments for large enterprises.
<BR><BR>
- <B>How are the GPU and CPU servers connected to each other?</B><BR>
Both the production and development environments are connected using Fibre Channel.

<BR><BR>
- <B>Shouldn't we make more use of client-side GPUs?</B><BR>
There are no game-specific programs installed on the client side, and while we currently use a versatile video format, we're not sure what the future holds.
<BR><BR>
- <B>In either case, 1:1, 1:N, or N:N, the actions will be input on the client PC side, but how do you plan to deal with environments where lag (communication delays) occur?</B><BR>
In Japan, we can ensure an optimal network environment by utilizing NGN. We aim for a maximum latency of 30ms to 40ms, and our rendering system has already cut the latency in half, creating a seamless environment.

<BR><BR>
- <B>How many players can each server accommodate? And how many servers are there in each data center?</B><BR>
The number of players that can be supported depends on the architecture used: a game using a 1:N architecture may support, say, 24 players, whereas a title using an N:N architecture could theoretically support 100 or more.
<BR><BR>
- <B>How will server maintenance and updates be handled? Will this impact end users?</B><BR>
Developers can work with Shinra Technology to easily deploy updates on their platforms. The process is designed to be as transparent as possible to end users, but the end-user impact may vary depending on the title and game architecture.
<BR><BR>
- <B>Is Shinra Technology's platform vulnerable to hacking, cheating, and piracy?</B><BR>
Because each gameplay session is streamed from a remote data center, the game code is stored on your hardware, so cheating is less of a concern, but it's still important to us.
<BR><BR>
- <B>What architecture should I use to build a cloud game?</B><BR>
Read the blog (link attached) to learn about the differences between the different types of Shinra Technology architectures classified as 1:1, 1:N, and N:N.
<BR><BR>
- <B>What is VCE and how do I use it?</B><BR>
VCE is a library written in C for high-speed inter-process communication using TCP sockets. It is very useful for writing N:N games for the Shinra platform. It runs on Windows, MacOS X and Linux. It was used as the network infrastructure in our sample game "Space Sweeper". VCE is free and open to the public, and is intended for use with CCDK.
<BR><BR>
- <B>Can I develop games with 1:1, 1:N, and N:N models using CCDK?</B><BR>
Yes, you can. There are no restrictions on the implementation model of your game.
<BR><BR>
- <B>What middleware is available for creating games with CCDK?</B><BR>
Middleware that uses DirectX 11 is supported by the system. However, it is the developer's responsibility to ensure that they comply with licensing and streaming restrictions (and how to work around them). We are currently working on a list of tested middleware.
<BR><BR>
- <B>What programming languages ​​can I use to develop my games?</B><BR>
Like "Space Sweeper", the sample programs of CCDK are written in C++. The API of the Shinra System is also written in C++. For other languages, as with the conditions for available middleware, any language that can be loaded as a DLL in Windows can be used. We will add those that have been verified to the compatibility and performance list, which is being prepared separately.
<BR><BR>
- <B>Is it possible to implement a back-end server on my own?</B><BR>
Yes, you can. There are also no restrictions on where you run your backend servers.
<BR><BR>
- <B>Can I use specific code for a particular GPU?</B><BR>
When programming for deployment on the Shinra platform, the code should be GPU-agnostic, as hardware specifications may vary slightly from data center to data center.
<BR><BR>
- <B>Is Shinra Technology compatible with Unity, Unreal, and GameMaker?</B><BR>
For 1:1 or N:N games, any engine or toolkit that utilizes DirectX 11 can be used with the Shinra architecture. For 1:N, special APIs must be used for graphics, sound, and manipulation. Each engine has different licensing agreements, and it is the developer's responsibility to comply with the licenses and other terms for streaming on the platform.
<BR><BR>
- <B>What multiplayer/network libraries can I use to develop N:N games?</B><BR>
There is no specific library required. Anything that can run multiple game instances on one machine and does not use UDP multicast is fine. VCE is officially supported.
<BR><BR>
- <B>Which controllers are supported by the Shinra platform?</B><BR>
Currently, the Shinra platform uses the traditional double analog stick as the primary input device. Keyboard and mouse input is also supported. If other devices are required, we will consider them on a case-by-case basis.
<BR><BR>
- <B>Does my game support UGC (user generated content) and/or mods?</B><BR>
Since the Shinra platform is based on remote rendering, there is the possibility of incorporating UGC into the title.
<BR><BR>
- <B>How can I sell games created with CCDK?</B><BR>
Our goal is to support independent developers using CCDK, and we will provide updates on the business side of CCDK development in future Q&As.
<BR><BR>
- <B>Can I self-publish on the Shinra platform?</B><BR>
Of course! The Shinra platform also welcomes self-publishing.
<BR><BR>
- <B>What is the submission process like, including getting it on the platform?</B><BR>
Games on the Shinra Platform require extensive QA before submission, after which they are subject to an approval process.
<BR><BR>
- <B>Can I run an online technical alpha test of my game with consumers?</B><BR>
We plan to provide private and public testing environments for prototype accelerators to CCDK developers.
<BR><BR>
- <B>Are Shinra Technology titles subject to rating?</B><BR>
The Shinra platform does not currently require rating approval, but all content must comply with our content policies, which will be published soon.
<BR><BR>
- <B>Can I publish previously published catalog titles on the Shinra platform?</B><BR>
The Shinra Platform is always looking for quality content, and will review past titles on a case-by-case basis.
<BR><BR>
- <B>Are Shinra Platform titles exclusivity?</B><BR>
No, there is no platform exclusivity, however the game is unique to the Shinra platform architecture and it is unlikely that the same experience will be recreated on other platforms.
<BR><BR>
- <B>Can I distribute or publish adult or sexually explicit content on the Shinra Platform?</B><BR>
No, Shinra Platform developers will not allow adult or sexually explicit content as we follow our content policy (content policy will be released soon).
<BR><BR>
- <B>How do I test Shinra Platform games locally?</B><BR>
Developers can run their games on Shinra MCS (Minimum Cloud Set), which acts as a wrapper for the Shinra platform, allowing users to start and stream their games over a local network for testing.
<BR><BR>