How to use a backend server
====

The backend server runs on Linux or MacOS X.
A communication server for implementing multiplayer games.

There are two modes: real-time mode and database mode.
A single process can provide functionality for both modes simultaneously.

It is implemented using VCE, and the communication protocol is defined in the ssproto.txt file.



## Source code
The source code is available in ```CCDK/backend```.
When you build it, two programs will be generated: ssv and ssbench.

The backend server source code is fully publicly available.
The actual C++ code, excluding the automatically generated part, is as follows:
It's a small program with just 2,000 lines in total, including comments, in main.cpp and presence.h.

Add features that are not implemented in the current version,
Feel free to delete unnecessary features, modify it as you like, and use it in your projects.

In addition, in the Shinra system, when scaling out game servers,
It is not necessary to use the backend server that comes with CCDK.


## ssv startup options

When you start ssv, the following explanation will be displayed:

~~~
bash$ ./ssv
Usage:
ssv realtime [OPTIONS]
ssv database [OPTIONS]
ssv realtime database [OPTIONS]
Options:
--dump-sp : dump shared projects with interval
--dump-p : dump projects with interval
--debug-protocol : abort on protocol parser error (for special debug case, dont use in production)
--maxcon=NUMBER : set max number. Absolute max is 200. Requires huge memory if maxcon gets larger.
--emulate-slow-disk=NUMBER : Enable slow disk emulation by milliseconds (sleep after each disk access)
--channel_max=NUMBER : set max concurrent number of channel members.
--tcp_timeout=SECONDS : set TCP timeout for database and realtime connections
--enable-fsync : Use fsync() when writing a static file (not affect on Redis storage)
--redis-addr HOSTNAME : Address of the redis server. Default is localhost
~~~

ssv requires at least one argument.
The following are all valid command lines:

```realtime``` or ```database``` specifies the operating mode,
You must specify either one or both.

~~~
bash$ ./ssv realtime
bash$ ./ssv database
bash$ ./ssv database realtime
bash$ ./ssv database realtime --maxcon=20
bash$ ./ssv database --maxcon=20 --redis-addr 192.168.1.181
bash$ ./ssv database realtime --maxcon=20 --dump-sp --dump-p
bash$ ./ssv database realtime --emulate-slow-disk=5
bash$ ./ssv database realtime --channel_max=300 --redis
bash$ ./ssv realtime --channel_max=300
~~~

The meaning of each option is as follows:

* ```--dump-sp``` This will periodically display a list of shared projects. This is for debugging purposes.
* ```--dump-p``` This will periodically display a list of projects hosted by the backend server. This is for debugging purposes.
--debug-protocol This option outputs detailed logs about received RPC functions. It generates a huge amount of logs, so it is dangerous to use it for anything other than development purposes.
* ```--maxcon=NUMBER``` Specifies the maximum number of connections. The setting limit is 200. The reason for the upper limit is the size of the memory buffer required for communication. (VCE allocates a fixed size initially.)
* ```--emulate-slow-disk=NUMBER``` This will emulate a slow disk, such as a spinning HDD, on the backend server, causing delays in database mode services, allowing you to see how this affects the game's performance.
* ```--channel_max=NUMBER``` Sets the maximum number of total channels that may be active simultaneously.
--tcp_timeout=SECONDS Sets the TCP timeout period. The default is 10 seconds.
--enable-fsync Issues an fsync() after every write to a static file. Does not affect operations on Redis.
--redis-addr HOSTNAME Specifies the location of the Redis server. The default value is localhost.


##How to fix ssproto.txt


In UNIX (Linux, MacOS X), you can easily edit ssproto.txt to add new commands.

The steps required to add a command are as follows:

1. Change the contents of ssproto.txt. For example, let's say you have defined a command to be issued from the client, ```=c2s foo( int bar )```.
2. Build. VCE's gen.rb will run and ssproto_sv.cpp, ssproto_sv.h, ssproto_cli.cpp, ssproto_cli.h, etc. will be generated.
3. I get a compilation error saying that the ssproto_foo_recv() function is undefined.
4. Define the entity of the ssproto_foo_recv() function on the server side.
5. Call the ssproto_foo_send() function on the client side to check that it works.

In the future, CCDK plans to make it possible to build on Windows if ssproto.txt is changed.

For detailed specifications of gen.rb, please refer to the VCE documentation.



