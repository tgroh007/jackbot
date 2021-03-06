# JackBot

A Discord bot for hosting JackBox Party Pack 4. 

## Details

This is not intended to be an official bot, this is just a project I threw together to help my family stay connected. There might be more sophisticated ways but this only took me 4-5 hours write.

### What it is

JackBot is a hacky way to get a bot running and streaming games inside Discord. This can be beneficial if players are not in the same location and certain hosts do not have access to decent internet or a computer to run the game. It is intended to be hosted by someone with a little tech knowledge so the non-tech-savvy folks can enjoy these games if we're not available.

### How it works

The premise is simple. Discord nor JackBox have native PowerShell integration so JackBot uses WScript to interact with the Windows and navigate around the apps. It has a limited state capacity to keep track of its position inside the app at all times.

### Does this work with other games

Technically it can work with any game that can be interacted with by keyboard commands. In its current form it only supports JackBox Party Pack 4 but all the other JackBox games could easily be supported. I don't own all of them or I would have added them. If there's enough interest I can look into the timings of the other games and add an app switcher fairly easily.

## Prereqs

You'll need a number of things to get this bot working correctly

* A Discord account that the bot will use to host the games
* A Discord server that you have Manage rights to so you can give the bot Read Message History permissions
* A Discord API Bot used to read text from channels
* OPTIONAL BUT RECOMMENDED: A secondary Steam account so the bot can run games without interfering with your personal account. You cannot run two games simultaneously
* A copy of Jackbox Party Pack 4, I run mine through Steam (other games can be used by adjusting keywords and timings)
* A Windows machine to host the games from

## Discord Account

Create a new [Discord Account](https://discordapp.com/register) that the bot can use for hosting services. This account doesn't require any special permissions.

## Discord Server

Create a new Discord server or use an existing one. The only requirement is that you have an account that has the ```Manage Server``` roll, this is required to add the bot later. You can create a new server with [this tutorial](https://support.discordapp.com/hc/en-us/articles/204849977-How-do-I-create-a-server-).

Copy the server name for later, we'll need it as ```DiscordServerName```

* Enter your server in Discord and select ```Server Settings``` from the drop down. You can do this from any account that has the ```Manage Server``` role.

![ServerSettings](https://user-images.githubusercontent.com/6700545/79265909-70a25800-7e5c-11ea-9dc1-622c0b5a545c.png)
* Select ```Webhooks``` from the left hand navigation menu.
* Select ```Create Webhook``` and fill out the details needed to proceed.

![WebHook](https://user-images.githubusercontent.com/6700545/79265934-7861fc80-7e5c-11ea-88e3-3b0f75bd2dfc.png)
* Give the Webhook bot a name, and icon if you wish. This is the primary name of the bot the users will see, so I named mine JackBot.
* Copy the ```Webhook URL``` at the bottom of the creation/edit page, we will need it later as ```DiscordHook```.

## Discord API Bot

The bot will remain in the channel but we're only using it to read messages. It's far easier to use webhooks for displaying messages.

* Create a new Application in the [Discord Development Portal](https://discordapp.com/developers/applications) and name it whatever you'd like.

![Application](https://user-images.githubusercontent.com/6700545/79265735-2e791680-7e5c-11ea-8030-254533dfaf5e.png)
* Copy the ```Client ID``` in the newly created application, we'll need it later as ```DiscordID```.

![BotID](https://user-images.githubusercontent.com/6700545/79265770-3afd6f00-7e5c-11ea-834b-f328dac406d8.png)
* Select ```Bot``` from the left hand navigation pane and copy the ```Token```, we'll need it later as ```DiscordToken```.

![BotToken](https://user-images.githubusercontent.com/6700545/79265794-42247d00-7e5c-11ea-9898-3b610079e0ab.png)
* Select ```OAuth2``` from the left hand navigation pane and select ```bot``` from the ```Scopes``` section.

![BotToScopes](https://user-images.githubusercontent.com/6700545/79265810-48b2f480-7e5c-11ea-8f0e-80cc7187ad53.png)
* Select the permissions for the bot, you need ```Read Message History``` at a minimum. You can add more here if you'd like to add functionality but I didn't need it.

![SelectReadMessages](https://user-images.githubusercontent.com/6700545/79265898-6b450d80-7e5c-11ea-99ee-3c81ad87173b.png)
* Scroll back up to the ```Scopes``` section and copy the URL at the bottom of ```Scopes```. It should read something like the following:
```https://discordapp.com/api/oauth2/authorize?client_id=<YOUR BOT ID>&permissions=<YOUR PERMISSION NUMBER>&scope=bot```
* Paste the above link into the browser and grant the bot permissions to the Discord server we made earlier. You will need to be logged in with an account that has the ```Manage Server``` role to the Discord server you're trying to add the bot to.

![AddBotToServer](https://user-images.githubusercontent.com/6700545/79265674-186b5600-7e5c-11ea-8b76-672cab33f673.png)

## Gathering the rest of the parameters

There are two more items you'll need to grab for the bot to work correctly

* In the Discord app as the user with ```Manage Server``` role, select settings at the bottom.

![DiscordSettings](https://user-images.githubusercontent.com/6700545/79265859-5f594b80-7e5c-11ea-9cdf-0d6d38bc8e86.png)
* Select ```Appearance``` from the left hand navigation menu and scroll down until you see ```Advanced```. ```Turn on Developer Mode```.

![DiscordDevMode](https://user-images.githubusercontent.com/6700545/79265845-59636a80-7e5c-11ea-88af-bd2d498b50bd.png)
* Navigate to the server that the bot has been added to, right click the text channel that you want the bot to listen in, and select ```Copy ID``` at the bottom. We'll need this later as ```DiscordTextChannelID```.

![CopyChannelID](https://user-images.githubusercontent.com/6700545/79265821-4fda0280-7e5c-11ea-8e33-1e40fa85817c.png)

## Windows Settings

I'm running my bot on a Windows 10 VM on a spare machine I have in my house. The VM is running in VirtualBox as a service. It's important that whatever box you run the bot from is not running much of anything else. Definitely DO NOT run it on your normal-use machine.

I won't go into setting up and running the VM, as that's outside the scope of the bot, but I'll give a few key details that need to be set if you're running inside a VM:

* 3D hardware acceleration.
* Access to Host audio.
* Enough CPU to run Jackbox and Discord stream (Discord stream hits the processor pretty good, but nothing most machines can't handle).
* 4GB of RAM recommended
* Access to the internet

No matter which machine you run it on, you need to ensure the following settings are set. These are well documented so I won't reinvent the wheel:

* Ensure Windows does not sleep
* (PowerShell) Set-ExecutionPolicy to unrestricted
* Disable the lock screen

You will need to install and log into the following items

* Log into the Steam account you want the bot to run under. Remember to check Remember Password
* Log into the Discord account you want the bot to stream from. Discord will stay logged in.
* Join the streaming account to the Discord server it will be streaming in.
* **IMPORTANT: In Discord, as the streaming account, go to ```Settings``` > ```Keybinds```. You MUST add a keybinding for ```Toggle Go Live Streaming``` and set the keybind to ```CTRL + ALT + L``` or the stream will never activate.**

![KeyBind](https://user-images.githubusercontent.com/6700545/79265875-65e7c300-7e5c-11ea-87ca-387ed55831c3.png)

## Configuring the bot

Once all of the above items are in line you should be ready to configure and run the bot. You'll need the items we've gathered up until now to fill out the config.

1. Download the files and unpack them to wherever you'd like the bot to run from. I've added path agnostic bat files so the location should not matter.
2. Once downloaded copy ```ConfigTemplate.JSON```, and rename it to ```Config.JSON```. 
3. Update ```DiscordID``` with ```DiscordID``` we gathered earlier.
Example: ```555555555555555555```
4. Update ```DiscordChannelName``` with the name of the channel the bot will be streaming to.
Example: ```general```
5. Update the ```DiscordServerName``` with the name of the Discord Server the bot will be streaming to.
Example: ```GameHost```
6. Update the ```DiscordTextChannelID``` with the ```DiscordTextChannelID``` we gathered earlier.
Example: ```555555555555555555```
7. Update the ```DiscordToken``` with the ```DiscordToken``` we gathered earlier.
Example: ```LKAJSHDLJAS_lakjshdliASD_olkjbsdlkhjASDlkjasbdjhGLKJ.DHblkasjd```
8. Update the ```DiscordHook``` with the ```DiscordHook``` we gathered earlier.
Example: ```https://discordapp.com/api/webhooks/555555555555555555/kljhasd098uasDPOIASD897asiudhkjhbasd0AS&d9*ASYdijbsad```
9. Update the ```DiscordLink``` with the path to the Discord executable. I placed a shortcut to the same folder in mine. Note that Windows paths need to be escaped in JSON. Discord has an auto-update feature so I suggest placing a shortcut to the same path and linking to that.
Example: ```C:\\Link\\To\\Folder\\Discord.lnk```
10. Update the ```JackboxLink``` with the path to the JackBox executable. I placed a shortcut to the same folder in mine. Note that Windows paths need to be escaped in JSON. Steam uses url links to launch games, I suggest placing a shortcut in the same path and linking to that.
Example: ```C:\\Link\\To\\Steam\\JackBox.url```
11. Create a Windows Task to run ```JackBot.bat``` inside the bot folder.
12. Double Click ```JackBot.bat``` to start the bot manually.
