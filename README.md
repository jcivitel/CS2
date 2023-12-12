[![Docker Pulls](https://img.shields.io/docker/pulls/jcivitell/cs2)](https://hub.docker.com/r/jcivitell/cs2) [![Docker Stars](https://img.shields.io/docker/stars/jcivitell/cs2)](https://hub.docker.com/r/jcivitell/cs2) [![Docker Image Size](https://img.shields.io/docker/image-size/jcivitell/cs2/latest)](https://hub.docker.com/r/jcivitell/cs2)


# Supported tags and respective `Dockerfile` links
-	[`latest` (*bullseye/Dockerfile*)](https://github.com/CM2Walki/CS2/blob/main/bullseye/Dockerfile)

# What is Counter-Strike 2?
For over two decades, Counter-Strike has offered an elite competitive experience, one shaped by millions of players from across the globe. And now the next chapter in the CS story is about to begin. This is Counter-Strike 2.
This Docker image contains the dedicated server of the game.

>  [CS2](https://store.steampowered.com/app/730/CounterStrike_2/)

<img src="https://cdn.cloudflare.steamstatic.com/steam/apps/730/header.jpg?t=1696011820" alt="logo" width="300"/></img>

# How to use this image
## Hosting a simple game server

**Initial one-time setup**

As of now, you can't download the CS2 dedicated server using `+login anonymous`, as it's part of the [CS2 client](https://steamdb.info/app/730).

1. [Create a fresh Steam account](https://store.steampowered.com/join/) and add CS2 to its library or use your own. [Optional if you already have an account]<br/>

2. Create required named volume:
```console
$ docker volume create steamcmd_login_volume # Location of login session
```

3. Activate the SteamCMD login session, if required enter your e-mail Steam Guard code (this will permanently save your login session in `steamcmd_login_volume`). Replace the following fields before executing the command:
- [STEAMUSER] - steam username
- [ACCOUNTPASSWORD] - steam account password
```console
$ docker run -it --rm \
    -v "steamcmd_login_volume:/home/steam/Steam" \
    cm2network/steamcmd \
    bash /home/steam/steamcmd/steamcmd.sh +login [STEAMUSER] [ACCOUNTPASSWORD] +quit
```

**Running a CS2 dedicated server**

4. Run using a bind mount for data persistence on container recreation. Replace the following fields before executing the command:
- [STEAMUSER] - steam username (no password required, if you completed step 1)
```console
$ mkdir -p $(pwd)/cs2-data
$ chmod 777 $(pwd)/cs2-data # Makes sure the directory is writeable by the unprivileged container user
$ docker run -d --net=host \
    -v $(pwd)/cs2-data:/home/steam/cs2-dedicated/ \
    -v "steamcmd_login_volume:/home/steam/Steam" \
    --name=cs2-dedicated -e STEAMUSER=[STEAMUSER] cm2network/cs2
```

**The container will automatically update the game on startup, so if there is a game update just restart the container.**

# Configuration
## Environment Variables
Feel free to overwrite these environment variables, using -e (--env):
```dockerfile
STEAMUSER="changeme"        (Steam user stored in steamcmd_login_volume)
CS2_SERVERNAME="changeme"   (Set the visible name for your private server)
CS2_PORT=27015              (CS2 server listen port tcp_udp)
CS2_LAN="0"                 (0 - LAN mode disabled, 1 - LAN Mode enabled)
CS2_RCONPW="changeme"       (RCON password)
CS2_PW="changeme"           (CS2 server password)
CS2_MAXPLAYERS=10           (Max players)
CS2_GAMETYPE=0              (Game type, see https://developer.valvesoftware.com/wiki/Counter-Strike_2/Dedicated_Servers)
CS2_GAMEMODE=1              (Game mode, see https://developer.valvesoftware.com/wiki/Counter-Strike_2/Dedicated_Servers)
CS2_MAPGROUP="mg_active"    (Map pool)
CS2_STARTMAP="de_inferno"   (Start map)
CS2_ADDITIONAL_ARGS=""      (Optional additional arguments to pass into cs2)
CS2_BOT_DIFFICULTY=""       (0 - easy, 1 - normal, 2 - hard, 3 - expert)
CS2_BOT_QUOTA=""            (Number of bots)
CS2_BOT_QUOTA_MODE=""       (fill, competitive)
```

# Credits

This repository is based on [https://github.com/CM2Walki/CSGO](https://github.com/CM2Walki/CSGO) (obsolete as of September 2023).<br/>
This repository is a fork of and uses code from [https://github.com/joedwards32/CS2](https://github.com/joedwards32/CS2).

# Contributors
[![Contributors Display](https://badges.pufler.dev/contributors/jcivitel/cs2?size=50&padding=5&bots=false)](https://github.com/CM2Walki/cs2/graphs/contributors)
