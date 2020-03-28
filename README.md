
# Installing the DST Server

```
sudo dpkg --add-architecture i386
```
## Digital Ocean
### this updates your new droplet
```
sudo apt-get update && time sudo apt-get dist-upgrade
```

### this installs needed dependencies to run the DST server
```
sudo apt-get install libstdc++6:i386 libgcc1:i386 libcurl4-gnutls-dev:i386
```

## Dedicated Linux
### For a 64-bit machine:
```
 sudo apt-get install libstdc++6:i386 libgcc1:i386 libcurl4-gnutls-dev:i386
```


### For a 32-bit machine:
```
sudo apt-get install libstdc++6 libgcc1 libcurl4-gnutls-dev
```

## DST Server related:
### this addes a new user named 'dst'
```
adduser dst
```

### this adds the new dst user to the sudo group
```
usermod -aG sudo dst
```

### move SSH Keys to user for ssh and sftp connection
```
rsync --archive --chown=dst:dst ~/.ssh /home/dst
```

### this changes active terminal account from 'root' to 'dst'
```
su dst
```

### this installs steamcmd
```
mkdir ~/steamcmd
```
```
cd ~/steamcmd
```
```
wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
```
```
tar -xvzf steamcmd_linux.tar.gz
```

### this makes folders for master and cave server shards
```
mkdir -p ~/.klei/DoNotStarveTogether/MyDediServer/Master
```
```
mkdir -p ~/.klei/DoNotStarveTogether/MyDediServer/Caves
```

### this creates your server token file, if you don't yet have your cluster token you'll need to create one here:
https://accounts.klei.com/account/game/servers?game=DontStarveTogether
```
echo 'enter your server token here' > ~/.klei/DoNotStarveTogether/MyDediServer/cluster_token.txt
```

### Create your cluster.ini file
```
base64 -di > ~/.klei/DoNotStarveTogether/MyDediServer/cluster.ini <<< 'W0dBTUVQTEFZXQpnYW1lX21vZGUgPSBzdXJ2aXZhbAptYXhfcGxheWVycyA9IDYKcHZwID0gZmFsc2UKcGF1c2Vfd2hlbl9lbXB0eSA9IHRydWUKCgpbTkVUV09SS10KY2x1c3Rlcl9kZXNjcmlwdGlvbiA9IFRoaXMgc2VydmVyIGlzIHN1cGVyIGR1cGVyIQpjbHVzdGVyX25hbWUgPSBTdXBlciBTZXJ2ZXIKY2x1c3Rlcl9pbnRlbnRpb24gPSBjb29wZXJhdGl2ZQpjbHVzdGVyX3Bhc3N3b3JkID0gCgoKW01JU0NdCmNvbnNvbGVfZW5hYmxlZCA9IHRydWUKCgpbU0hBUkRdCnNoYXJkX2VuYWJsZWQgPSB0cnVlCmJpbmRfaXAgPSAxMjcuMC4wLjEKbWFzdGVyX2lwID0gMTI3LjAuMC4xCm1hc3Rlcl9wb3J0ID0gMTA4ODkKY2x1c3Rlcl9rZXkgPSBzdXBlcnNlY3JldGtleQo='
```

### Create your Master server.ini
```
base64 -di > ~/.klei/DoNotStarveTogether/MyDediServer/Master/server.ini <<< 'W05FVFdPUktdCnNlcnZlcl9wb3J0ID0gMTEwMDAKCgpbU0hBUkRdCmlzX21hc3RlciA9IHRydWUKCgpbU1RFQU1dCm1hc3Rlcl9zZXJ2ZXJfcG9ydCA9IDI3MDE4CmF1dGhlbnRpY2F0aW9uX3BvcnQgPSA4NzY4Cg=='
```

### Create your Caves server.ini
```
base64 -di > ~/.klei/DoNotStarveTogether/MyDediServer/Caves/server.ini <<< 'W05FVFdPUktdCnNlcnZlcl9wb3J0ID0gMTEwMDEKCgpbU0hBUkRdCmlzX21hc3RlciA9IGZhbHNlCm5hbWUgPSBDYXZlcwoKCltTVEVBTV0KbWFzdGVyX3NlcnZlcl9wb3J0ID0gMjcwMTkKYXV0aGVudGljYXRpb25fcG9ydCA9IDg3NjkK'
```

### Create your Caves worldgenoverride.lua
```
base64 -di > ~/.klei/DoNotStarveTogether/MyDediServer/Caves/worldgenoverride.lua <<< 'cmV0dXJuIHsKICAgIG92ZXJyaWRlX2VuYWJsZWQgPSB0cnVlLAogICAgcHJlc2V0ID0gIkRTVF9DQVZFIiwKfQo='
```

### Create the script that will run the servers.
```
base64 -di > ~/run_dedicated_servers.sh <<< 'IyEvYmluL2Jhc2gKCnN0ZWFtY21kX2Rpcj0iJEhPTUUvc3RlYW1jbWQiCmluc3RhbGxfZGlyPSIkSE9NRS9kb250c3RhcnZldG9nZXRoZXJfZGVkaWNhdGVkX3NlcnZlciIKY2x1c3Rlcl9uYW1lPSJNeURlZGlTZXJ2ZXIiCmRvbnRzdGFydmVfZGlyPSIkSE9NRS8ua2xlaS9Eb05vdFN0YXJ2ZVRvZ2V0aGVyIgoKZnVuY3Rpb24gZmFpbCgpCnsKICAgICAgICBlY2hvIEVycm9yOiAiJEAiID4mMgogICAgICAgIGV4aXQgMQp9CgpmdW5jdGlvbiBjaGVja19mb3JfZmlsZSgpCnsKICAgIGlmIFsgISAtZSAiJDEiIF07IHRoZW4KICAgICAgICAgICAgZmFpbCAiTWlzc2luZyBmaWxlOiAkMSIKICAgIGZpCn0KCmNkICIkc3RlYW1jbWRfZGlyIiB8fCBmYWlsICJNaXNzaW5nICRzdGVhbWNtZF9kaXIgZGlyZWN0b3J5ISIKCmNoZWNrX2Zvcl9maWxlICJzdGVhbWNtZC5zaCIKY2hlY2tfZm9yX2ZpbGUgIiRkb250c3RhcnZlX2Rpci8kY2x1c3Rlcl9uYW1lL2NsdXN0ZXIuaW5pIgpjaGVja19mb3JfZmlsZSAiJGRvbnRzdGFydmVfZGlyLyRjbHVzdGVyX25hbWUvY2x1c3Rlcl90b2tlbi50eHQiCmNoZWNrX2Zvcl9maWxlICIkZG9udHN0YXJ2ZV9kaXIvJGNsdXN0ZXJfbmFtZS9NYXN0ZXIvc2VydmVyLmluaSIKY2hlY2tfZm9yX2ZpbGUgIiRkb250c3RhcnZlX2Rpci8kY2x1c3Rlcl9uYW1lL0NhdmVzL3NlcnZlci5pbmkiCgouL3N0ZWFtY21kLnNoICtmb3JjZV9pbnN0YWxsX2RpciAiJGluc3RhbGxfZGlyIiArbG9naW4gYW5vbnltb3VzICthcHBfdXBkYXRlIDM0MzA1MCB2YWxpZGF0ZSArcXVpdAoKY2hlY2tfZm9yX2ZpbGUgIiRpbnN0YWxsX2Rpci9iaW4iCgpjZCAiJGluc3RhbGxfZGlyL2JpbiIgfHwgZmFpbCAKCnJ1bl9zaGFyZWQ9KC4vZG9udHN0YXJ2ZV9kZWRpY2F0ZWRfc2VydmVyX251bGxyZW5kZXJlcikKcnVuX3NoYXJlZCs9KC1jb25zb2xlKQpydW5fc2hhcmVkKz0oLWNsdXN0ZXIgIiRjbHVzdGVyX25hbWUiKQpydW5fc2hhcmVkKz0oLW1vbml0b3JfcGFyZW50X3Byb2Nlc3MgJCQpCgoiJHtydW5fc2hhcmVkW0BdfSIgLXNoYXJkIENhdmVzICB8IHNlZCAncy9eL0NhdmVzOiAgLycgJgoiJHtydW5fc2hhcmVkW0BdfSIgLXNoYXJkIE1hc3RlciB8IHNlZCAncy9eL01hc3RlcjogLycKCgo='
```

### Update the ``cluster.ini`` File in `` klei/DontStarveTogether/MyDediServer``
```ini
[GAMEPLAY]
game_mode = endless
max_players = 16
pvp = false
pause_when_empty = true


[NETWORK]
cluster_description = Yet another Gameserver
cluster_name = workhard-playhard
cluster_intention = cooperative
cluster_password = workhard-playhard


[MISC]
console_enabled = true


[SHARD]
shard_enabled = true
bind_ip = 127.0.0.1
master_ip = 127.0.0.1
master_port = 10889
cluster_key = supersecretkey
```

### Give the script executable permissions
```
chmod u+x ~/run_dedicated_servers.sh
```

### Install all relevant server files with:
```
~/run_dedicated_servers.sh
```

Caution: Keep the terminal open, the installation process will be terminated if you close the terminal window!
If everything wen't fine, go in-game and shut down the server with:

```
c_shutdown(true)
```

## Add mods
Upload files from `` dontstarvetogether_dedicated_server/mods`` to the same folder on your server

## Upload Mods modifications
Upload files from `` klei/DontStarveTogether/MyDediServer`` via sftp to ``.klei/DontStarveTogether/MyDediServer``

```
.klei/DoNotStarveTogether/MyDediServer
```

### You can also use this information:
https://gist.github.com/madeleine-neumann/3133d23bc27d6c7e0237bb7f90784264

### Remove this line from ``run_dedicated_servers.sh`` to install mods and re-add it after installation for updates
```
./steamcmd.sh +force_install_dir "$install_dir" +login anonymous +app_update 343050 validate +quit
```

### Run the script to start the dedicated servers
```
nohup ~/run_dedicated_servers.sh &
```
Press
```
ctrl a and ctrl d
```
to detach from terminal


### Backups
``crontab -e``

If you would like to make backups of your different servers, it could possibly look like this:

#### Content of crontab file: 
```
0 * * * * tar -czf /home/dst/`date +\%Y-\%m-\%d_\%H-\%M`survival_backup.tar.gz -C /home/dst/.klei/DoNotStarveTogether SurvivalServer
0 * * * * tar -czf /home/dst/`date +\%Y-\%m-\%d_\%H-\%M`endless_backup.tar.gz -C /home/dst/.klei/DoNotStarveTogether EndlessServer
0 * * * * tar -czf /home/dst/`date +\%Y-\%m-\%d_\%H-\%M`endless_friends_backup.tar.gz -C /home/dst/.klei/DoNotStarveTogether EndlessFriends
```





