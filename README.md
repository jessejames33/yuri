<div align="center">
     <img src="http://zekro.de/ss/yurilogo.png" width="200"/>
     <h1>~ Yuri ~</h1>
     <strong>If you believe or not, the best discord soundboard in whole north korea!<br>And it can be controlled with HOTKEYS or over the WEBINTERFACE!</strong><br><br>
     <img src="https://forthebadge.com/images/badges/made-with-javascript.svg" height="30" />&nbsp;
     <img src="https://forthebadge.com/images/badges/uses-html.svg" height="30" />&nbsp;
     <img src="https://forthebadge.com/images/badges/uses-css.svg" height="30" />&nbsp;
     <a href="https://zekro.de/discord"><img src="https://img.shields.io/discord/307084334198816769.svg?logo=discord&style=for-the-badge" height="30"></a>
     <br>
     <br>
     <a href="https://travis-ci.org/zekroTJA/yuri" ><img src="https://travis-ci.org/zekroTJA/yuri.svg?branch=master" /></a>&nbsp;
     <a href="https://github.com/zekroTJA/yuri/releases"><img src="https://img.shields.io/github/tag/zekroTJA/yuri.svg" /></a>
</div>

---

Branch | Status
-------|--------
Master | <a href="https://travis-ci.org/zekroTJA/yuri/branches" ><img src="https://travis-ci.org/zekroTJA/yuri.svg?branch=master" /></a>
Dev    | <a href="https://travis-ci.org/zekroTJA/yuri/branches" ><img src="https://travis-ci.org/zekroTJA/yuri.svg?branch=dev" /></a>
Master @ HotkeyClient | <a href="https://travis-ci.org/zekroTJA/yuri.hotkeyclient/branches"><img src="https://travis-ci.org/zekroTJA/yuri.hotkeyclient.svg?branch=master" /></a>

---

## Commands

**Command** | **Aliases** | **Description**                                                      
|----|----|----|                                                                                 
| `<sound>` | -/- | Play a sound |                                                                 
| bind `<sound>`/r/reset | -/- | bind a sound or random sounds to the fast key, "reset" to reset | 
| disable | enable | disable or enable the soundboatd [OWNER ONLY] |                             
| help | -/- | Display this help message |                                                       
| info | -/- | Display some info about this bot |                                                
| list `[s]` | ls | List all sounds (add argument "s" for time-sorted list) |                      
| log | logs, history | display guilds sound history |                                           
| quit | -/- | Quit current voice channel |                                                      
| random | r, rand | Play a random sound |                                                       
| search `<query/regexp>` | s | search for sounds by query or regexp |                             
| stop | -/- | Stop currently playing sound (does not quit channel) |                            
| summon | -/- | Summons the bot in the voice channel without playing a sound |

---

## What you need

- Node.JS and npm
- FFMPEG

**OR**

- docker

---

## Setup
Register a Discord Bot and its OAuth2 [**here**](http://discordapp.com/developers/applications/me). In OAuth2 add a redirect to `<serveraddress>/authorize` (i.e. `http://zekro.de:6612/authorize`) where `<serveraddress>` is the same one in `config.json`!

There are **2 options** for self-hosting yuri:

1. [With the supplied Docker image](#1---using-the-supplied-docker-image)
2. [With manual node.js setup configuration](#2---manual-configuration-of-your-nodejs-setup) *(way more complex)*

### 1 - Using the supplied Docker image

Go to [**Releases**](https://github.com/zekroTJA/yuri/releases) and download the latest Docker image of yuri.  
The image contains:  
    - an image of node.js LTS (8.12.0) on Debian Jessie  
    - the source code of yuri  
    - a FFMPEG installation  

i) load the image to docker with
```
$ sudo docker load -i yuri.dockerimage.tar
```

ii) create a directory `expose` with the folder `sounds` in it with
```
$ mkdir -p expose/sounds
```

iii) start the Docker container once to create the config file in the expose folder
```
$ sudo docker run -v $PWD/expose:/usr/src/app/expose yuri
```

iv) a `config.json` should have been created in the `expose` directory. Open the config and enter your Bot application token from [**here**](http://discordapp.com/developers/applications/me). Also enter your preffered prefix the bot will listen to, your ID to identify you as the bot owner and the websocket password/token. Set the sound files directory to `"./expose/sounds"`.

v) you can put all your sounds into the `expose/sounds` directory.

vi) run the Docker container as deamon with the prefered port of the websocket expose:
```
$ sudo docker run -p 8080:6612 -v $PWD/expose:/usr/src/app/expose -d yuri
```
*`8080` is just an example. You can expose the web interface to whatever port you want. Keep in mind, that the paths on the left side of the `:` behind the `-v` tag **always** needs to be an absolute path!*

Now, you can check the status of the container with
```
sudo docker ps
```

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
1bdf62abc781        yuri                "bash runner.sh"    19 hours ago        Up 19 hours         0.0.0.0:6612->6612/tcp   fervent_carson
```

To stop the container, use the stop command with the container ID:
```
$ sudo docker stop 1bdf62abc781
```

### 2 - Manual configuration of your node.js setup

> Why you need to do all this stuff? Simply, the sound library of yuri has some problems with later versions of node.js than 9.4.0. So, you need to install all dependencies on this version and run the bot with it.

i) Clone the repository
```
$ git clone https://github.com/zekroTJA/yuri.git && cd yuri
```

ii) Install **ffmpeg** on your system by following the steps described in [**this blog post**](https://superuser.com/questions/286675/how-to-install-ffmpeg-on-debian#865744).


iii) Install npm package `n`, which will be used to manage multiple node.js versions
```
$ sudo npm i -g n
```

iv) Install node 9.4.0 and check that the version has changed
```
$ sudo n 9.4.0
$ node -v
v9.4.0
```

v) Install all node modules
```
$ npm i
```

vi) Open the `package.json` file and change the `"start"` command to `"n use 9.4.0 src/main.js"`
```js
"scripts": {
  "start": "n use 9.4.0 src/main.js"
},
```

vii) Start the bot once to create the `config.json` located in the `expose` folder.
```
$ npm start
```

viii) Open the config and enter your Bot application token from [**here**](http://discordapp.com/developers/applications/me). Also enter your preffered prefix the bot will listen to, your ID to identify you as the bot owner and the websocket password/token. Set the sound files directory where you want to store your sound files. 
Attention: This directory will **not** be created automatically by bot, so you will need to create the directory manually!

ix) start the bot with the `runner.sh` script to ensure that the bot restarts after using the restart function. In the best case, run all of this in a screen session
```
$ screen -dmLS yuri bash runner.sh
```
*If you do not want to log the output of the screen, omit the `L` argument.*

x) change back to your latest version of node
```
$ sudo n latest
```
*...or enter the version you prefer to use normally.*

---

## Web Interface

If you set a valid token in the config, you can log in to the web interface accessed through `http://<ipoftheserver>:6612` *or with the port you set to if you changed it.*

Then, join a voice channel in a guild the bot is connected to and enter your token and user ID. More information can be found in the info field.

![](http://zekro.de/ss/chrome_2018-07-04_17-37-54.png)

After logging in, you can play sounds by clicking the tiles, search for sounds, or manage the bot.

![](http://zekro.de/ss/2018-08-20_19-15-06.gif)

---

## Hotkey Client

**[Here](https://github.com/zekroTJA/yuri.hotkeyclient)** you will find more information about setting up the websocket and getting the hotkey client.

---

## To Do

[**Here**](https://github.com/zekroTJA/yuri/projects/1) you can find current tasks to do and ideas to implement.

---

## Used 3rd party dependencies

- [body-parser](https://www.npmjs.com/package/body-parser)
- [cookie-parser](https://www.npmjs.com/package/cookie-parser)
- [colors](https://www.npmjs.com/package/colors)
- [discord.js](https://www.npmjs.com/package/discord.js)
- [express](https://www.npmjs.com/package/express)
- [express-handlebars](https://www.npmjs.com/package/express-handlebars)
- [node-opus](https://www.npmjs.com/package/node-opus)
- [socket.io](https://www.npmjs.com/package/socket.io)
- [request](https://www.npmjs.com/package/request)
- [sqlite3](https://www.npmjs.com/package/sqlite3)
- [sha256](https://www.npmjs.com/package/sha256)

---

© 2018 zekro Development (Ringo Hoffmann)  
[zekro.de](https://zekro.de) | contact[at]zekro.de
