---
layout: post
title: Current State of Modded Valheim Server
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

Once the game files have been downloaded save this command for late within a script. It can be run at a later time to update the game whenever the Valheim team releases a new version. I saved mine under `~/bin/update-valheim`. If you leave out the file extension, as I have done, you can add the script to your path and run the scrip as if it were a built-in command in your shell. Add this to the bottom of your `.bashrc` file:

```bash
# adding custom scripts within the ~/bin directory to function simillar to built-in commands
export PATH=$PATH:~/bin
```
{: file="~/.bashrc" }

If followed exactly you can type `update-valheim` into your terminal from anywhere and the __steamcmd__ command will run to update the game.

## Important File Structure


