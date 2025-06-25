Shinra Development Station (SDS) Overview ====Shinra Development Station (SDS) is a tool for managing Shinra game projects. The main purpose of SDS is to manipulate Shinra project files.

Project files are used to test your game locally, create packages for your game,
It is used to deploy to the cloud.


Basic Concepts ----The SDS GUI consists of three elements: ![](images/sds_gui.png) At the top of the screen is the menu bar, which allows you to save and load projects, start the game, and access the settings screen.

On the left side of the screen is the project tree, which displays information about the project you are currently working on.
A project can contain several data packs and startup configurations, the contents of which are described below. By clicking on each element in the project tree you can open the relevant properties screen on the right.

The large right-hand section of the screen displays detailed configuration information related to the currently selected element in the Project Tree.


## Configuring SDS itself

The settings of the SDS tool itself can be checked by selecting MCS configuration from Settings in the menu bar (Figure). ![](images/sds_mcs_configuration.png)
**Python executable** is the path to ``python.exe``. Python 3 is required. CCDK development uses 3.4.

**Shinra script path** is the path to ```shinra.py```. This script is used internally by SDS to create packages, install games, run them, etc. By default, it is stored in a subdirectory of the directory where SDS is located.

**Shinra MCS path** is the directory location where the files such as DLLs other than SDS of MCS are stored. Normally, MCS is provided in the same compressed file as SDS, so you can simply specify the directory where SDS is located. When creating a package, SDS will complete the package by copying DLLs from this directory.

**Games installation dir** is the working directory that SDS uses when installing the game to the local environment. A separate directory for each project will be created in the directory specified here. Since it contains a copy of the entire game data, it needs to have sufficient capacity.

**Default game user id** Specifies the default value for the username required when connecting from the Shinra client.

If you leave **Force overwriting of game data on install** unchecked, only files that have actually changed (size or date differs) will be copied from among the files required for the game. It is unchecked by default. This can reduce the installation time for games with large data.


## Structure of the Shinra Project
The Shinra Project consists of two parts: a **Data Pack** and a **Startup Setup**.

A data pack defines the configuration of files and directories required for the game to run.
The startup settings define how the game will be launched and what the MCS settings will be.

File locations in a startup configuration are specified relative to the root directory within a data pack, so a valid startup configuration must identify a data pack (a project can contain several).

A typical procedure is to first define a data pack, then define the startup settings, specifying an executable file (exe file) included in the data pack as the startup file.

Please also refer to [Setup.jamd](Setup.ja.md) for specific examples of settings.


### Create a data pack

The data pack included in the Shinra Project can be used by simply specifying the location of the data.
The data itself is not included. To include the data itself, you need to create a Shinra Pack. This will be explained later.

There are three ways to create a data pack:

- From the Project menu, click Add data pack.
- Right-click on ``Data packs``` in the project tree and select ``Add data pack```
- Drag and drop the directory from Explorer into the project tree.

When you create a data pack, the following items become available for configuration.
- **Id** An ID to distinguish this data pack from other data packs. Specify any string.
- **Version** The version number used to track the data pack. This is useful for debugging.
- **Directory** Specifies the directory location from which to copy the data.
- **Alias** Specifies the name of the directory the data will actually be extracted to. By default this will be the same as the source directory, but you can set it to any name.

### Create a startup configuration

You can create a startup configuration in three ways:

- In the ```Proejct``` menu, click ```Add Startup configuration```.
- Right-click on ``Startup configurations``` in the project tree and select ``Add Startup configuration```
- In the file listing for each data pack, right-click on the executable and select ``Add startup configuration``

![](images/sds_add_startup.png)Once you add a startup setting, you can configure the following items.




- **Id** An ID to distinguish this from other startup settings. Enter any string you like.
- **Executable** The path to the executable in the data pack (relative to the root).
- **Arguments** Command line arguments to use when executing
- **Work directory** The working directory (relative to the root) to use when executing.
- **Data pack** The data pack to use, it must be defined in the project.
- **Save data** Specify the file to be automatically saved when the game server program ends. Write a list of statements to filter the file path or a file hook. The next time the same user runs this game, the previously saved file will be automatically written. This makes it easy to implement game save data.
- **Temp data** Location of temporary data. When the game server program is terminated, the files and directories in the path (or file hook) specified here will be automatically deleted.

For more information about file hooks, see [MCS_README.ja.md](MCS_README.ja.md).

You can conveniently drop files from the Datapack preview window into the text field, and you can also assign more than one startup setting to one executable file.


## Running the game on MCS
You can use MCS to run your games locally.

### Starting the game
You can start the game in two ways.

- From the ```Project``` menu, select ```Start game``` and choose the startup configuration you want to start. - From the project tree, right-click the startup configuration you want to start and select ```Start game```.

When you start a game, the game will be copied into the game installation directory ```Games installation dir``` set in the SDS itself, the necessary DLLs etc. will be automatically installed, and the game will start there.
Once the installation is complete, the game execution management window will appear.

When starting a game, the game data will be deployed in the directory specified under the "MCS configuration", and then executed there. Once the installation is over you will be prompted the Game running window.

### Game execution management window

When you select ```Start game``` from the project menu, the following window will appear. ![](images/sds_two_instances.png)You can start multiple games from one startup setting.
Each row in this window corresponds to one running instance of the game.
An instance is a single game server process in Windows.
For example, if you want to try three-player multiplayer, you'll need three instances and three lines of this setting.

The contents displayed on each line are as follows:


- **User id** Specifies the ID of the user to be used in the instance. A single user ID cannot be shared across multiple instances. Please specify a different user ID for each instance.
- **Game port** Specify the TCP port number of the game port. The game port is the TCP port used to send operation information from the Shinra client to the game server. This port number must not be occupied by other programs.
- **Video port** Specify the TCP port number of the video port. The video port is the TCP port for the Shinra client to receive video data. This port number must not be occupied by other programs.
- **Game** This button starts/stops the game. It starts the game with the user ID, port number, and other settings set in the instance. When the game is ready to start, it is ```Start```, and once started, it changes to ```Stop``` button.
- **Client** This button starts the Shinra client (video viewer). It uses the game port and video port settings set in the instance to properly configure and launch the client. By default, the client will automatically launch when you launch the game. If you want to connect and test play from another computer, uncheck this option.

You can add a new instance by right-clicking on the white space in the instance list and selecting ```Add game instance```. You can also delete an instance by selecting it and pressing the delete key.



## Shinra Pack
Projects can be easily packaged.
A package is a single zip file that contains everything needed to run the game (executables, data, etc.).
Packaging allows you to archive your game, share it among team members, or upload it to Shinra Technology's data centers for deployment.

### Creating a Shinra Pack
Simply select ```Build ShinraPack``` from the ```Project``` menu.
Please confirm that the zip file is created in the specified location.


## Loading the Shinra Pack

SDS normally loads Shinra project files (with the extension .shinra). Unlike Shinra packs, project files do not include exe files.

However, since the Shinra Pack contains project information, SDS can load the Shinra Pack and resume editing the project contents.

To import a Shinra pack, select ```Import project``` from the ```File``` menu and enter the following information: - ```Import from packages``` The location of the zip file to import - ```Store data in``` Where to extract the project data
- Project file: Name of the project file

