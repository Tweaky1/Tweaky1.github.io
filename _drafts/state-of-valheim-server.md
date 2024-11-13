---
layout: post
title: Installing Valheim Server and Running It as a systemd Service
date: 2024-11-11 20:00:00 +-0000
image: /assets/img/preview/valheim-preview.png
description: Short tutorial to get a Valheim server up and running on linux and a reminder to myself of how my current Valheim server installation is setup.
categories: [Reminders]
tags: [systemd,bash,games]
---

## Services and Required Knowledge

[__steamcmd__](https://developer.valvesoftware.com/wiki/SteamCMD) is a command line tool required to manage any and all steam games you wish to host locally on a linux based server, including Valheim. Ensure it is installed before proceeding. If you do not see steamcmd listed in the output of the first command use the second to install it.

> Examples are assuming you have a Debian based linux distro using the `apt` package manager. Please use your provided distrobution's package manager and its required syntax.
{: prompt-info }

```bash
sudo apt list --installed | grep steamcmd

sudo apt install steamcmd -y
```

Each steam game is identified on steam with a unique identifier called an "_App ID_," or colloquially a, "_Steam ID_." You will need to find the proper ID and pass that into __steamcmd__ when requesting to download or update the game. Becareful as you will want to ensure you are using the Steam ID for Valheim's dedicated server version of the game, not the standard game installed on your gaming PC. You can find any Steam ID you need simply from searching google, or you can use the [steamdb](https://steamdb.info/) website. Valheim Server's Steam ID is: `896660`.

Once you have __steamcmd__ installed and have the necessary Steam ID you can run the command below to download the game files:

> For ease of use later on with systemd setup use a path with __NO__ spaces or special characters.
{: .prompt-warning }

```bash
steamcmd +login anonymous +force_install_dir /PATH/YOU/WANT/GAME/FILES +app_update 896660 validate +exit
```

Once the game files have been downloaded save this command for late within a script. It can be run at a later time to update the game whenever the Valheim team releases a new version. I saved mine under `~/bin/update-valheim`. Mark your file as executable with `chmod` and you can stop there if you like. If you leave out the file extension, as I have done, you can add the script to your path and run the script as if it were a built-in command in your shell. Add this to the bottom of your `.bashrc` file:

```bash
# adding custom scripts within the ~/bin directory to function simillar to built-in commands
export PATH=$PATH:~/bin
```
{: file="~/.bashrc" }

If followed exactly you can type `update-valheim` into your terminal from anywhere and the __steamcmd__ command will run to update the game.

## Important File Structure

All of your important game files will be stored under the path you specify within the initial command for the download. e.g. Mine are stored under `~/steam/steamapps/common/valheim`, this holds all important binaries and other assets required to run the game. The save files for your world, along with lists of players administrator privlages in game and those who may be banned are found in `~/.config/unity3d/IronGate/Valheim`.

Should you want to install mods to improve upon and change the vanilla Valheim experience in anyway; that will require the installation of [__BepInEx__](https://docs.bepinex.dev/articles/user_guide/installation/index.html?tabs=tabid-nix) on top of your Valheim install. You can download the files form either the __BepInEx__ [website](https://docs.bepinex.dev/index.html), or from the [GtiHub](https://github.com/BepInEx/BepInEx/releases) page for the project. I have found the GitHub page more straight forward to follow. Simply download the `.zip` file designated for your operating system and subsequent architecture. After download unzip the file and copt the contents of the unzipped file to the root of your game istallation. Following my own installation that is `~/steam/steamapps/common/valheim`.

> At the time of writing the steps for installation when downloading BepInEx from their website seem to differ slightly. I have never followed this route. If you wish too please follow the documentation listed there.
{: prompt-info }

To add mods to your server the configuration files can be found under `~/steam/steamapps/common/valheim/BepInEx`. In here you will find a `plugins` folder for storing specific binaries and additional files required to get mods up and running. Each individual mod will need its own folder to hold those files required for the mod to function. Using my installation as an example the path will look something like this: `~/steam/steamapps/common/valheim/BepInEx/plugins/FORLDER-FOR-MOD/MOD-FILE(s)`. Typically every individual mod is packaged as a `.zip` file and taking the output folder from unzipping the file and placing it in the mods folder will suffice. Conversely, 99% of mods only consist of a single `.dll` file that can simply sit in the plugins folder. For ease of use (one maybe two less clicks) I unzip the downloaded file and drop the outputed folder into the `plugins` directory on the server. 


